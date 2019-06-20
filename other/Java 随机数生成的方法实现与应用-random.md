---
title: Java 随机数生成的方法实现与应用-random
date: 2015-09-05 13:04:07
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/48224469   
  static void setSeed(long seed) 设置随机生成器的种子   
 /**_**********************************_**   
 随机数中用到。每次的Seed不同，random就不同了。   
 在进行随机时，随机算法的起源数字称为种子数(seed)，   
 在种子数的基础上进行一定的变换，从而产生需要的随机数字。   
 **_***********************************_**/

 static double random() 0到1之间的实数   
 static int uniform(int N) 0到N-1之间的整数   
 static int uniform (int lo,int hi) lo到hi之间的整数   
 static double uniform(double lo, double hi) lo到hi之间的实数   
 static bollean bernoulli(double p) 返回真的概率是p   
 static double gaussian() 正态分布,期望值为0,标准差为1   
 static double gaussian(double m,double s) 正态分布,期望值为m,标准差为s   
 static int iscrete(double [] a) 返回i的概率为a[i]   
 static void shuffle(double [] a) 将数组a随机排序

 -

 
## 使用random实现一些数据

 随机返回[a,b)之间的一个double值 return a+StdRandom.random()*(b-a);   
 随机返回[0…..N]之间的一个int值 return (int)StdRandom.random()*N;   
 随机返回[lo,hi) 之间的一个int值 returned lo+StdRandom.uniform(hi-lo);   
 根据离散概率随机返回int 的值

 
```
 //a[]中个元素之和必须的呢关于1
 double r=StdRandom.random();
 double sum=0.0;
 for(int i=0;i<a.length;i++){
 sum=sum+a[i];
 if(sum>=r) return i;
 }
 return -1;
 }

```
 随机将double 数组中的元素排序

 
```
int N=a.length;
for(int i=0;i<N;i++){
int r=i+rStdRandom.uniform(N-i);
double temp=a[i];
a[i]=a[r];
a[r]=temp;
}
}
```
   
  