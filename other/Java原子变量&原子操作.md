---
title: Java原子变量&原子操作
date: 2017-12-05 22:13:35
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/78725608   
  很多情况下我们只是需要一个简单的、高效的、线程安全的递增递减方案。注意，这里有三个条件：简单，意味着程序员尽可能少的操作底层或者实现起来要比较容易；高效意味着耗用资源要少，程序处理速度要快；线程安全也非常重要，这个在多线程下能保证数据的正确性。这三个条件看起来比较简单，但是实现起来却难以令人满意。

 通常情况下，在Java里面，++i或者–i不是线程安全的，这里面有三个独立的操作：

 
```
 1.获得变量当前值
 2.为该值+1/-1
 3.然后写回新的值
```
 在没有额外资源可以利用的情况下，只能使用**加锁**才能保证读-改-写这三个操作是“原子性”的。

 Java 5新增了**AtomicInteger类**，该类包含方法getAndIncrement()以及getAndDecrement()，这两个方法实现了原子加以及原子减操作，但是比较不同的是这两个操作没有使用任何加锁机制，属于无锁操作。 

 在JDK 5之前Java语言是靠**synchronized关键字**保证同步的，这会导致有锁（后面的章节还会谈到锁）。

 
```
  锁机制存在以下问题：

  （1）在多线程竞争下，加锁、释放锁会导致比较多的上下文切换和调度延时，引起性能问题。

  （2）一个线程持有锁会导致其它所有需要此锁的线程挂起。

  （3）如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置，引起性能风险。

```
 volatile是不错的机制，但是volatile不能保证原子性。因此对于同步最终还是要回到锁机制上来。

 独占锁是一种悲观锁，synchronized就是一种独占锁，会导致其它所有需要锁的线程挂起，等待持有锁的线程释放锁。而另一个更加有效的锁就是乐观锁。所谓乐观锁就是，每次不加锁而是假设没有冲突而去完成某项操作，如果因为冲突失败就重试，直到成功为止。

 **CAS 操作**

 上面的乐观锁用到的机制就是CAS，Compare and Swap。

 CAS有3个操作数，内存值V，旧的预期值A，要修改的新值B。当且仅当预期值A和内存值V相同时，将内存值V修改为B，否则什么都不做。

 **非阻塞算法 （nonblocking algorithms）**

 一个线程的失败或者挂起不应该影响其他线程的失败或挂起的算法。

 现代的CPU提供了特殊的指令，可以自动更新共享数据，而且能够检测到其他线程的干扰，而 compareAndSet() 就用这些代替了锁定。

 拿出AtomicInteger来研究在没有锁的情况下是如何做到数据正确性的。

 
```
private volatile int value;
```
 首先毫无疑问，在没有锁的机制下需要借助volatile原语，保证线程间的数据是可见的（共享的），这样获取变量值的时候才能直接读取。

 
```
public final int get() {
        return value;
    }
```
 然后来看看++i是怎么做到的。

 
```
public final int incrementAndGet() {
    for (;;) {
        int current = get();
        int next = current + 1;
        if (compareAndSet(current, next))
            return next;
    }
}
```
 在这里采用了CAS操作，每次从内存中读取数据然后将此数据和+1后的结果进行CAS操作，如果成功就返回结果，否则重试直到成功为止。

 而compareAndSet利用JNI来完成CPU指令的操作。

 
```
public final boolean compareAndSet(int expect, int update) {   
    return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    }
```
 整体的过程就是这样子的，利用CPU的CAS指令，同时借助JNI来完成Java的非阻塞算法。其它原子操作都是利用类似的特性完成的。

 而整个J.U.C都是建立在CAS之上的，因此对于synchronized阻塞算法，J.U.C在性能上有了很大的提升。参考资料的文章中介绍了如果利用CAS构建非阻塞计数器、队列等数据结构。

 CAS看起来很爽，但是会导致“ABA问题”。

 CAS算法实现一个重要前提需要取出内存中某时刻的数据，而在下时刻比较并替换，但是在这个时间差内任何变化都可能发生。

 比如说一个线程one从内存位置V中取出A，这时候另一个线程two也从内存中取出A，并且two进行了一些操作变成了B，然后two又将V位置的数据变成A，这时候线程one进行CAS操作发现内存中仍然是A，然后one操作成功。尽管线程one的CAS操作成功，但是不代表这个过程就是没有问题的。如果链表的头在变化了两次后恢复了原值，但是不代表链表就没有变化。要解决”ABA问题”，我们需要增加一个版本号，在更新变量值的时候不应该只更新一个变量值，而应该更新两个值，分别是变量值和版本号，AtomicStampedReference支持在两个变量上进行原子的条件更新，可以使用该类进行更新操作。

 

 
--------
 

 
> 以下为您介绍下Java中新增的原子更新类型
> 
>  
 
--------
 
## 原子更新基本类型

 使用原子方式更新基本类型，共包括3个类：

 AtomicBoolean：原子更新布尔变量   
 AtomicInteger：原子更新整型变量   
 AtomicLong：原子更新长整型变量   
 具体到每个类的源代码中，提供的方法基本相同，这里以AtomicInteger为例进行说明。AtomicInteger提供的部分方法如下：

 
```
public class AtomicInteger extends Number implements java.io.Serializable {
    //返回当前的值
    public final int get() {
        return value;
    }
    //原子更新为新值并返回旧值
    public final int getAndSet(int newValue) {
        return unsafe.getAndSetInt(this, valueOffset, newValue);
    }
    //最终会设置成新值
    public final void lazySet(int newValue) {
        unsafe.putOrderedInt(this, valueOffset, newValue);
    }
    //如果输入的值等于预期值，则以原子方式更新为新值
    public final boolean compareAndSet(int expect, int update) {
        return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    }
    //原子自增
    public final int getAndIncrement() {
        return unsafe.getAndAddInt(this, valueOffset, 1);
    }
    //原子方式将当前值与输入值相加并返回结果
    public final int getAndAdd(int delta) {
        return unsafe.getAndAddInt(this, valueOffset, delta);
    }
}
```
 为了说明AtomicInteger的原子性，这里代码演示多线程对一个int值进行自增操作，最后输出结果，代码如下：

 
```
package com.rhwayfun.concurrency.r0405;

import java.util.concurrent.atomic.AtomicInteger;

/**
 * Created by rhwayfun on 16-4-5.
 */
public class AtomicIntegerDemo {

    private static AtomicInteger atomicInteger = new AtomicInteger(0);

    public static void main(String[] args){
        for (int i = 0; i < 5; i++){
            new Thread(new Runnable() {
                public void run() {
                    //调用AtomicInteger的getAndIncement返回的是增加之前的值
                    System.out.println(atomicInteger.getAndIncrement());
                }
            }).start();
        }
        System.out.println(atomicInteger.get());
    }
}
```
 输出结果如下：

 
```
0 
1 
2 
3 
4 
5 
5
```
 可以看到在多线程的情况下，得到的结果是正确的，但是如果仅仅使用int类型的成员变量则可能得到不同的结果。这里的关键在于getAndIncrement是原子操作，那么是如何保证的呢？

 getAndIncrement方法的源码如下：

 
```
    public final int getAndIncrement() {
        return unsafe.getAndAddInt(this, valueOffset, 1);
    }

    public final int getAndAddInt(Object var1, long var2, int var4) {
        int var5;
        do {
            var5 = this.getIntVolatile(var1, var2);
        } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));

        return var5;
    }

    public final native boolean compareAndSwapInt(Object var1, long var2, int var4, int var5);
```
 到这里可以发现最终调用了native方法来保证更新的原子性。

 
## 原子更新数组

 通过原子更新数组里的某个元素，共有3个类：

 AtomicIntegerArray：原子更新整型数组的某个元素   
 AtomicLongArray：原子更新长整型数组的某个元素   
 AtomicReferenceArray：原子更新引用类型数组的某个元素   
 AtomicIntegerArray常用的方法有：

 int addAndSet(int i, int delta)：以原子方式将输入值与数组中索引为i的元素相加   
 boolean compareAndSet(int i, int expect, int update)：如果当前值等于预期值，则以原子方式更新数组中索引为i的值为update值   
 示例代码如下：

 
```
  package com.rhwayfun.concurrency.r0405;

    import java.util.concurrent.atomic.AtomicIntegerArray;

    /**
     * Created by rhwayfun on 16-4-5.
     */
    public class AtomicIntegerArrayDemo {

    static int[] value = new int[]{1, 2};

    static AtomicIntegerArray ai = new AtomicIntegerArray(value);

    public static void main(String[] args){
        ai.getAndSet(0,3);
        System.out.println(ai.get(0));
        System.out.println(value[0]);
    }
}
```
 运行结果是:

 
```
3 
1
```
 数组value通过构造的方式传入AtomicIntegerArray中，实际上AtomicIntegerArray会将当前数组拷贝一份，所以在数组拷贝的操作不影响原数组的值。

 
## 原子更新引用类型

 需要更新引用类型往往涉及多个变量，早atomic包有三个类：

 AtomicReference：原子更新引用类型   
 AtomicReferenceFieldUpdater：原子更新引用类型里的字段   
 AtomicMarkableReference：原子更新带有标记位的引用类型。   
 下面以AtomicReference为例进行说明：

 
```
package com.rhwayfun.concurrency.r0405;

import java.util.concurrent.atomic.AtomicIntegerFieldUpdater;

/**
 * Created by rhwayfun on 16-4-5.
 */
public class AtomicIntegerFieldUpdaterDemo {

    //创建一个原子更新器
    private static AtomicIntegerFieldUpdater<User> atomicIntegerFieldUpdater =
            AtomicIntegerFieldUpdater.newUpdater(User.class,"old");

    public static void main(String[] args){
        User user = new User("Tom",15);
        //原来的年龄
        System.out.println(atomicIntegerFieldUpdater.getAndIncrement(user));
        //现在的年龄
        System.out.println(atomicIntegerFieldUpdater.get(user));
    }

    static class User{
        private String name;
        public volatile int old;

        public User(String name, int old) {
            this.name = name;
            this.old = old;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getOld() {
            return old;
        }

        public void setOld(int old) {
            this.old = old;
        }
    }
}
```
 输出的结果如下：

 
```
15 
16
```
 至此，我们知道了如何使用原子操作类在不同场景下的基本用法。

   
  