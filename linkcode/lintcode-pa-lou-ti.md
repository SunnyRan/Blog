---
title: LintCode-爬楼梯
date: '2015-11-08T21:56:27.000Z'
tags: CSDN迁移
---

# LintCode-爬楼梯

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49722333](https://blog.csdn.net/Sunny_Ran/article/details/49722333)  
爬楼梯

假设你正在爬楼梯，需要n步你才能到达顶部。但每次你只能爬一步或者两步，你能有多少种不同的方法爬到楼顶部？

样例  
比如n=3，1+1+1=1+2=2+1=3，共有3中不同的方法

返回 3

登上第1级：1种  
登上第2级：2种  
登上第3级：1+2=3种（前一步要么从第1级迈上来,要么从第2级迈上来）  
登上第4级：2+3=5种（前一步要么从第2级迈上来,要么从第3级迈上来）  
登上第5级：3+5=8种  
登上第6级：5+8=13种  
登上第7级：8+13=21种  
登上第8级：13+21=34种  
登上第9级：21+34=55种  
登上第10级：34+55=89种．

```text
//递归版本
public static int climbStairs(int n) {
        int re = 0;
        if(n == 0 || n == 1){
            return 1;
        }else{
            re = climbStairs(n-1)+climbStairs(n-2);
        }
        return re;
    }

//非递归版本
    public static int climbStairs(int n){
        int a = 1, b = 1, re = 0;
        if(n == 1 || n ==0){
            return 1;
        }
        while(--n > 0){
            re = a+b;
            b = a;
            a = re ;        
        }
        return re;
    }
```

