---
title: Memcache的并发问题和利用CAS的解决方案
date: 2017-08-17 13:59:56
tags: CSDN迁移
---
  首先来描述下Memeche遇到的一个简单的并发问题，原来MEMCACHED中的Keys的内容为A，客户端C1和客户端C2都把A取了出来，C1往准备往其中加B，C2准备往其中加C，这就会造成C1和C2执行后的CACHE KEYS要么是AB要么是AC，而不会出现我们期望的ABC。这种情况，如果不是在集群环境中，而只是单机服务器，可以通过在写CACHE KEYS时增加同步锁，就可以解决问题，可是在集群环境中，JVM内部的同步锁是显然解决不了问题的，那怎么办呢？

 首先来看下，Memcache是原子的吗？宏观上所有的被发送到Memcache的单个命令是完全原子的。如果您针对同一份数据同时发送了一个set命令和一个get命令，它们不会影响对方。它们将被串行化、先后执行。

 然而，由于不同的客户端发送的命令到达服务器的顺序是不可控的，甚至由于Memcache服务器使用多线程，即使是同一个客户端发送的两个命令，他们在服务器端的处理顺序也是不可控的。

 所以，命令序列不是原子的，在多线程模式，即使所有的命令都是原子的，命令序列不是原子的。如果您通过get命令获取了一个item，修改了它，然后想把它set回memcached，我们不保证这个item没有被其他进程（process，未必是操作系统中的进程）操作过。这就是著名的fetch-modify-set导致的并发问题。在并发的情况下，您也可能覆写了一个被其他进程set的item，但是这个覆写可能就产生了竞跑条件，就导致了过期的数据被更新了。

 Memcached 1.2.5以及更高版本，提供了gets和cas命令，它们可以解决上面的问题。如果您使用gets命令查询某个key的cache会给您返回该item当前值的唯一标识。如果您覆写了这个item并想把它写回到Memcached中，您可以通过cas命令把那唯一标识送给 Memcached。如果该Item存放在Memcached中的唯一标识与您提供的一致，您的写操作将会成功。如果另一个进程在这期间也修改了这个Item，那么该Item存放在Memcached中的唯一标识将会改变，您的写操作就会失败。

 Memcached保存的key/value都有一个唯一标识casUnique，在进行incr/decr操作时，首先获取casUnique，执行incr，检验返回值是否casUnique+1，如果是，则更新，否则，失败不更新！

 Memcache中的Add操作也是原子的，Add操作要不成功要不失败，失败了就是其他线程或者应用已经添加了此key/value.

 到现在，我们总结一下Memecache支持的多线程同步的方法，1. Add命令 2. incr/decr命令 3. gets/cas操作   
 现在我们回到当初的问题上来，我们怎么解决呢？不同的并发应用在串A上增加B和C，得到的结果期待是ABC或者ACB而不是AB或者AC呢，那么就需要一些同步方法来保护关键数据：

 最初的解决方案是计划利用memcached的add操作的原子性来控制并发，具体方式如下：

 1.申请锁：在校验是否创建过活动前，执行add操作key为memberId，如果add操作失败，则表示有另外的进程在并发的为该memberId创建活动，返回创建失败，否则表示无并发。   
 2.执行创建活动   
 3.释放锁：创建活动完成后，执行delete操作，删除该memberId。

 Java代码如下：

 
```
if (memcache.get(key) == null) {  
    // 3 min timeout to avoid mutex holder crash  
    if (memcache.add(key_mutex, 3 * 60 * 1000) == true) {  
        value = db.get(key);  
        memcache.set(key, value);  
        memcache.delete(key_mutex);  
    } else {  
        sleep(50);  
        retry();  
    }  
}  

```
 如此实现存在一些问题：

 1.Memcached中存放的值有有效期，即过期后自动失效，如add过M1后，M1失效，可以再次add成功。   
 2.即使通过配置，可以使Memcached永久有效，即不设有效期，Memcached有容量限制，当容量不够后会进行自动替换，即有可能add过M1后，M1被其他key值置换掉，则再次add可以成功。   
 3.此外，memcached是基于内存的，断电后数据会全部丢失，导致重启后所有memberId均可重新add。   
 针对上述的几个问题，根本原因是add操作有时效性，过期，被替换，重启，都会是原来的add操作失效。

 Memcached中除了add操作是原子的，还有另外两个操作也是原子的：incr和decr ，使用CAS模式即：

 1.预先在memcached中设置一个key值，假设为CREATKEY＝1   
 2.每次创建活动时，在规则校验前先get出CREATEKEY＝x；   
 3.进行规则校验   
 4.执行incr CREATEKEY操作，检验返回值是否为所期望的x＋1，如果不是，则说明在此期间有另外的进程执行了incr操作，即存在并发，放弃更新。否则   
 5.执行创建活动   
 这种方法可能存在误判，即本来不存在并发，却被判为并发，如缓存重启，或key值失效后，incr值可能不同于期望值，导致误判。

 下面使用incr/decr也可以提供同样的解决方案，但是也会遇到同样的问题，就是incr的标志变量过期，被清除等：

 
```
   if (memcache.incr(key_mutex, 3 * 60 * 1000) == 1) {  
        value = db.get(key);  
        memcache.set(key, value);  
        memcache.decr(key_mutex);  
    } else {  
        sleep(50);  
        retry();  
    }  

```
 所以解决该问题的最好办法是减轻时效性的影响，使用memcached CAS（check and set）方式。

 CAS基本原理非常简单，一言以蔽之，就是“版本号”，在数据库领域也叫乐观锁。每个存储的数据对象，都有一个版本号。我们可以从下面的例子来理解：

 如果不采用CAS，则有如下的情景：

 第一步，A取出数据对象X；   
 第二步，B取出数据对象X；   
 第三步，B修改数据对象X，并将其放入缓存；   
 第四步，A修改数据对象X，并将其放入缓存。   
 我们可以发现，第四步中会产生数据写入冲突，也就是写入了A持有的过期数据 。

 如果采用CAS协议，则是如下的情景。

 第一步，A取出数据对象X，并获取到CAS-ID1；   
 第二步，B取出数据对象X，并获取到CAS-ID2；这里CAS-ID2等于CAS-ID1;   
 第三步，B修改数据对象X，在写入缓存前，检查CAS-ID与缓存空间中该数据的CAS-ID是否一致。结果是“一致”，就将修改后的带有CAS-ID2的X写入到缓存。   
 第四步，A修改数据对象Y，在写入缓存前，检查CAS-ID与缓存空间中该数据的CAS-ID是否一致。结果是“不一致”，则拒绝写入，返回存储失败。   
 而使用memcached的CAS方式，可以以几乎0成本的方式解决时效性问题，尽管存在一点小缺陷，但这种缺陷可以通过简单的重试即可解决。考虑实际的产出比，采用memcached的CAS方式更适合实际情况。

 由于需要使用cas方法，Java的memcache客户端不支持该方法，所以改用Spy Memcache客户端 。

 下面代码没有使用CAS，因此会出现Fetch-Modify-Set的并发问题：

 
```
s='X';
```
 客户端1

 
```
String s = memcache.get(key);
memcache.set(key, s + 'A');
```
 客户端2

 String s = memcache.get(key);   
 memcache.set(key, s + ‘B’);   
 结果不是XAB或者XBA，是XA或者XB, 出现了get then update的竞跑条件。

 下面代码是使用了CAS的简单示例：

 cas, 命令gets/cas, javamemcacheclient没有api，spymemcache支持

 
```
CasValue cv = memcache.cas(key);
if (cv is null) {
    boolean b = cv.add(key, 0);
    if (!b) {
         CasValue cv = memcache.cas(key); 

          boolean r = false;
          while (!r)
                   r = cv.setValue(cv.getValue() + 1);
    }
} else {
          boolean r = false;
          while (!r)
                   r = cv.setValue(cv.getValue() + 1);
}
```
 下面是一个更加复杂的SpyMemcache使用CAS的案例：

 
```
package util.memcache.atomic;

import java.io.IOException;
import java.util.concurrent.ExecutionException;

import net.spy.memcached.CASResponse;
import net.spy.memcached.CASValue;
import net.spy.memcached.MemcachedClient;
import net.spy.memcached.internal.OperationFuture;

public class AtomicUpdater {

public static void main(String[] args) throws IOException,
InterruptedException, ExecutionException {
  MemcachedClient mc = null;
  // TODO design a MemcacheAtomicUpdater to make a callback interface
  addOrPad(mc, "B");
  addOrPad(mc, "C");
}

/**
* true: pad is added or deleted successfully false: key was deleted
*/
private static boolean addOrPad(MemcachedClient mc, String suffix)
throws InterruptedException, ExecutionException {
  final int retryTimes = 10;

  CASValue casValue = mc.gets("key");

  if (casValue == null) {
    OperationFuture f = mc.add("key", 0, "A");
    if (!f.get()) {
      return tryPad(mc, "key", suffix, retryTimes);
    }
    return true;
  } else {
    return tryPad(mc, "key", suffix, retryTimes);
  }
}

/**
*
* true: pad is successful false: key was deleted
*
* */
private static boolean tryPad(MemcachedClient mc, String key,
String suffix, int times) {
  int curr = 0;
  while (curr < times) {
    CASValue casValue = mc.gets(key);
    if (casValue == null)
      return false;

    CASResponse res = mc.cas(key, casValue.getCas(),
    casValue.getValue() + suffix);

    if (CASResponse.NOT_FOUND.equals(res))
      return false;
    else if (CASResponse.OK.equals(res)) {
      return true;
    }

    curr++;
  }
  return false;
}

}
```
 除了这个实例以外，我们还在项目中遇到一个类似的问题，在线的生产系统经常会遇到计数系统，例如点攒的计数系统，每一个Feed都有一个点攒的总数，还有一个点攒的列表，那么我们需要把热点Feed的点攒总数和列表保存在缓存中，那么问题就来了，点攒系统在前端机的集群中是并发执行的，就会并发的更新这个列表，如果不进行并发保护，那么就会和上面案例中出现相同的问题，就是会丢失列表中的某些元素，也就是在更新了A，B，C后列表变成了AB或者AC，那么同样可以采用CAS的方式进行更新数据，避免数据的丢失。关于点攒系统，请参考我的另外一篇博客，你可以从云博主页菜单找到《点赞模块的设计与实现》

 总结：

  
  2. Memcache的add, incr/decr, gets/cas都是原子的，如果遇到线程并发问题首先考虑使用gets/cas。
     
      
  4. Memcache client for java不支持CAS，可以使用add来解决，SpyMemcache和XMemcache支持CAS。
     
      
  6. Memcache遇到的主要并发问题是Fetch-Modify-Set产生的。
     
      
  8. 存数据库也会遇到同样的情况，但是数据库锁起来会更方便 。
     
         
  