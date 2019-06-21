---
title: LintCode- 最长上升连续子序列
date: '2015-11-08T23:13:25.000Z'
tags: CSDN迁移
---

# LintCode- 最长上升连续子序列

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49724767](https://blog.csdn.net/Sunny_Ran/article/details/49724767)  
最长上升连续子序列

给定一个整数数组（下标从 0 到 n-1， n 表示整个数组的规模），请找出该数组中的最长上升连续子序列。（最长上升连续子序列可以定义为从右到左或从左到右的序列。）

样例  
给定 \[5, 4, 2, 1, 3\], 其最长上升连续子序列（LICS）为 \[5, 4, 2, 1\], 返回 4.

给定 \[5, 1, 2, 3, 4\], 其最长上升连续子序列（LICS）为 \[1, 2, 3, 4\], 返回 4.

注意  
time

```text
public class Solution {
    /**
     * @param A an array of Integer
     * @return  an integer
     */
    public int longestIncreasingContinuousSubsequence(int[] A) {
           int max = 1,count = 1;
           if(A.length <=0){
               return A.length;
           }
           //正向遍历
           for(int i = 1; i<A.length; ){
               while(i<A.length && A[i]>A[i-1] ){
                   i++;
                   count++;
               }if(count > max){
                   max = count;
               }
               count = 1;
               i++;
           }
           //反向遍历
           for(int i = A.length - 1 ; i >= 0; ){
               while(i > 0 && A[i]<A[i-1]  ){
                   i--;
                   count++;
               }if(count > max){
                   max = count;
               }
               count = 1;
               i--;
           }


           return max;

       }
}
```

