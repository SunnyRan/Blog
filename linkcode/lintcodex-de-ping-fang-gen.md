---
title: LintCode-x的平方根
date: '2015-10-01T16:22:37.000Z'
tags: CSDN迁移
---

# LintCode-x的平方根

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/48846719](https://blog.csdn.net/Sunny_Ran/article/details/48846719)  
x的平方根

实现 int sqrt\(int x\) 函数，计算并返回 x 的平方根。

样例  
sqrt\(3\) = 1

sqrt\(4\) = 2

sqrt\(5\) = 2

sqrt\(10\) = 3

```text
 二分法。 我们知道假使y表示x的平方根的值，那么可以确定0<=y<x 的， 所以我们可以设一个min=0,max=x,然后每次用二分的方法求得mid=(min+max)/2，然后只要比较mid^2跟x比较，如果mid^2<x，那么max=mid继续二分，反之mid^2>x ,那么min=mid继续二分。直到找到一个mid使得它的平方最接近x就好。
```

```text
class Solution {
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
      public static int sqrt(int x) {
            long a=0,b=x,c=b/2;
            while(b-a>1){
                if(c*c<x){
                    a=c;
                }else{
                    b=c;
                }
                c=(b+a)/2;
            }
            return (int) (b*b>x?a:b);
        }
}
```

