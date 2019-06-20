---
title: Coin Change 2-硬币问题
date: 2017-03-07 23:03:01
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/60783153   
  **Coin Change 2 Add to List**   
 You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.

 Note: You can assume that

 0 <= amount <= 5000   
 1 <= coin <= 5000   
 the number of coins is less than 500   
 the answer is guaranteed to fit into signed 32-bit integer

 Example 1:

 Input: amount = 5, coins = [1, 2, 5]   
 Output: 4   
 Explanation: there are four ways to make up the amount:   
 5=5   
 5=2+2+1   
 5=2+1+1+1   
 5=1+1+1+1+1

 Example 2:

 Input: amount = 3, coins = [2]   
 Output: 0   
 Explanation: the amount of 3 cannot be made up just with coins of 2.   
 Example 3:

 Input: amount = 10, coins = [10]   
 Output: 1

 Example 3:

 Input: amount = 10, coins = [10]   
 Output: 1

 题意解释：给你不同面额的硬币和一个总金额，试着写出一个方法计算出可以凑成总金额的排列组合数。你可以假设每一种面额的硬币无限。

 方法一：   
 第一个方法是递归计算   
 但是在LeetCode上，计算 coins = {3,5,7,8,9,10,11};amount = 500；时，超时了。

 
```
    //计数器，记录可行的方案
    public int total = 0;
    //递归函数入口
    public int change(int amount, int[] coins) {
        if(amount == 0 )return 1;
        getChange(amount, coins, coins.length - 1);
        return total;
    }
    //递归函数
    public void getChange(int amount, int[] coins, int star) {
        if (star < 0) return;//star记录的coins[]开始的位置
        if (amount - coins[star] == 0) total++;
        if (amount - coins[star] > 0)
        {
            getChange(amount - coins[star], coins, star); //如果amount-coinsp[star]>0，则可以递归计算Fn(amount - coins[star])的值。
        }
        getChange(amount, coins, --star);
    }
```
 方法二:   
 dp[n]表示当总金额为n,有dp[n]种组合方法，那么对于问题来说，假设对于coins[i-1]与dp[n]已知，此时加入新的硬币面值coins[i],则if(n+1>=coins[i]) dp[n+1]+=dp[n+1-coin]

 这里以Example 1 为例子 

 Input: amount = 5, coins = [1, 2, 5]   
 Output: 4   
 Explanation: there are four ways to make up the amount:   
 5=5   
 5=2+2+1   
 5=2+1+1+1   
 5=1+1+1+1+1

 
```
/*
[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1] 当只有一种面额为1时，dp[]的值
[1, 1, 2, 2, 3, 3, 4, 4, 5, 5, 6]当有两种面额为1，2时，dp[]的值
[1, 1, 2, 2, 3, 4, 5, 6, 7, 8, 10]当有三种面额为1，2，5时，dp[]的值
*/
```
 
```
public int change(int amount, int[] coins) {
        int [] dp = new int[amount+1];
        dp[0] =1;
        for(int coin:coins){
            for(int i = 1;i<=amount;i++){
                if(i>=coin)dp[i]+=dp[i-coin];
            }
        }
        return dp[amount];
    }
```
   
  