---
title: Java中创建线程的方法以及区别
date: 2018-05-09 01:06:57
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/80247994   
  Java使用Thread类代表线程，所有的线程对象都必须是Thread类或其子类的实例。Java可以用多种方式来创建线程，如下所示：

 1）继承Thread类创建线程

 2）实现Runnable接口创建线程

 3）使用Callable和Future创建线程

 4） 通过线程池创建线程

 下面让我们分别来看看创建线程的方法。   
   
  
  


 
## 继承Thread类创建线程

 
--------
 通过继承Thread类来创建并启动多线程的一般步骤如下

 1.定义Thread类的子类，并重写该类的run()方法，该方法的方法体就是线程需要完成的任务，run()方法也称为线程执行体。

 2.创建Thread子类的实例，也就是创建了线程对象

 3.启动线程，即调用线程的start()方法

 
```
public class MyThread extends Thread{//继承Thread类

　　public void run(){

　　//重写run方法

　　}

}

public class Main {

　　public static void main(String[] args){

　　　　new MyThread().start();//创建并启动线程

　　}

}
```
 
## 实现Runnable接口创建线程

 
--------
 通过实现Runnable接口创建并启动线程一般步骤如下：

 1.定义Runnable接口的实现类，一样要重写run()方法，这个run()方法和Thread中的run()方法一样是线程的执行体

 2.创建Runnable实现类的实例，并用这个实例作为Thread的target来创建Thread对象，这个Thread对象才是真正的线程对象

 3.第三部依然是通过调用线程对象的start()方法来启动线程

 
```
public class MyThread2 implements Runnable {//实现Runnable接口

　　public void run(){

　　//重写run方法

　　}

}

public class Main {

　　public static void main(String[] args){

　　　　//创建并启动线程

　　　　MyThread2 myThread=new MyThread2();

　　　　Thread thread=new Thread(myThread);

　　　　thread().start();

　　　　//或者    new Thread(new MyThread2()).start();

　　}
}
```
 
## 使用Callable和Future创建线程

 
--------
 **和Runnable接口不一样，Callable接口提供了一个call（）方法作为线程执行体，call()方法比run()方法功能要强大。**

  
  * call()方法可以有返回值 
  * call()方法可以声明抛出异常  Java5提供了Future接口来代表Callable接口里call()方法的返回值，并且为Future接口提供了一个实现类FutureTask，这个实现类既实现了Future接口，还实现了Runnable接口，因此可以作为Thread类的target。在Future接口里定义了几个公共方法来控制它关联的Callable任务。

 
```
boolean cancel(boolean mayInterruptIfRunning)   
\\视图取消该Future里面关联的Callable任务

V get() 
\\返回Callable里call（）方法的返回值，调用这个方法会导致程序阻塞，必须等到子线程结束后才会得到返回值

V get(long timeout,TimeUnit unit)
 \\返回Callable里call（）方法的返回值，最多阻塞timeout时间，经过指定时间没有返回抛出TimeoutException

boolean isDone() 
\\若Callable任务完成，返回True

boolean isCancelled()
\\如果在Callable任务正常完成前被取消，返回True
```
 介绍了相关的概念之后，创建并启动有返回值的线程的步骤如下：

 1.创建Callable接口的实现类，并实现call()方法，然后创建该实现类的实例（从java8开始可以直接使用Lambda表达式创建Callable对象）。

 2.使用FutureTask类来包装Callable对象，该FutureTask对象封装了Callable对象的call()方法的返回值

 3.使用FutureTask对象作为Thread对象的target创建并启动线程（因为FutureTask实现了Runnable接口）

 4.调用FutureTask对象的get()方法来获得子线程执行结束后的返回值

 
```
public class Main {

　　public static void main(String[] args){

　　　 Callable<Integer> callable = new Callable<Integer>() {
            public Integer call() throws Exception {
                return new Random().nextInt(100);
            }
        };
        FutureTask<Integer> future = new FutureTask<Integer>(callable);
        new Thread(future).start();
        try {
            Thread.sleep(5000);// 可能做一些事情
            System.out.println(future.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

　　}

}
```
 
## 三种创建线程方法对比

 
--------
 实现Runnable和实现Callable接口的方式基本相同，不过是后者执行call()方法有返回值，后者线程执行体run()方法无返回值，因此可以把这两种方式归为一种这种方式与继承Thread类的方法之间的差别如下：

 1、线程只是实现Runnable或实现Callable接口，还可以继承其他类。

 2、实现Runnable和实现Callable接口，多个线程可以共享一个target对象，非常适合多线程处理同一份资源的情形。

 3、但是编程稍微复杂，如果需要访问当前线程，必须调用Thread.currentThread()方法。

 4、继承Thread类的线程类不能再继承其他父类（Java单继承决定）。

 注：一般推荐采用实现接口的方式来创建多线程

 

 
## 共享target对象测试

 
--------
 
##### 1.继承Thread类

 
```
public class TestThread extends Thread {
  int count = 3;

  public TestThread(String ThreadName) {
    super(ThreadName);
  }

  @Override
  public void run() {
    for (int i = 0; i < 10; i++)
      if (count > 0) {
        System.out.println(Thread.currentThread().getName() + "--->" + count);
        count--；
      }
  }

  public static void main(String[] args) {
    //new三个线程并启动
    new TestThread("线程一").start();
    new TestThread("线程二").start();
    new TestThread("线程三").start();
  }
}
```
 
##### 2.实现Runnable接口

 
```
public class TestThread implements Runnable {
  int count = 3;

  public TestThread() {
  }

  @Override
  public void run() {
    for (int i = 0; i < 10; i++)
      if (count > 0) {
        System.out.println(Thread.currentThread().getName() + "--->" + count);
        count--；
      }
  }

  public static void main(String[] args) {
    TestThread tr = new TestThread();
    //new三个线程并启动同一个Runnable
    new Thread(tr, "线程一").start();
    new Thread(tr, "线程二").start();
    new Thread(tr, "线程三").start();
  }
}
```
 
##### 输出结果

 
```
输出结果1(继承Thread类):              输出结果2(实现Runnable接口)：
线程一--->3                          线程一--->3
线程一--->2                          线程二--->2
线程一--->1                          线程一--->1
线程二--->3
线程二--->2
线程二--->1
线程三--->3
线程三--->2
线程三--->1
```
 可以发现两种新建线程的方式最后的输出结果不一样，是因为在继承Thread类中，同时创建了三个线程，每个线程都执行一个任务，相当于三个线程分别各自进行三次循环打印log；而在第二种实现Runnable接口中是创建三个Thread共同去执行tr这个Runnable，相当于三个Thread共同去执行这一个循环，使得最后count只循环了一次，剩余线程二和线程三都因为使用同一个count导致未能打印出来。

 
##### 结论

 1）两种创建线程的实现方式不一样，一个通过继承一个通过实现接口，在Java中如果已经继承了其他的父类，那么只能实现接口来创建线程。

 2）通过上面的例子可以看到继承Thread，每个线程都独立拥有一个对象，而实现Runnable对象，多个线程共享一个Runnable实例。

   
  