---
title: LintCode-打劫房屋
date: '2016-03-09T16:30:41.000Z'
tags: CSDN迁移
---

# LintCode-打劫房屋

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/50836441](https://blog.csdn.net/Sunny_Ran/article/details/50836441)  
打劫房屋  
假设你是一个专业的窃贼，准备沿着一条街打劫房屋。每个房子都存放着特定金额的钱。你面临的唯一约束条件是：相邻的房子装着相互联系的防盗系统，且 当相邻的两个房子同一天被打劫时，该系统会自动报警。

给定一个非负整数列表，表示每个房子中存放的钱， 算一算，如果今晚去打劫，你最多可以得到多少钱 在不触动报警装置的情况下。

样例

给定 \[3, 8, 4\], 返回 8.

```text
//分析：这是一个动态规划问题 在i位置取最大值时 只与在i-1，i—2 位置取得最大值有关
//分为两种情况
//1.第一种就是包含i位置的值，及最大值为i位置的值加在i-2位置取得的最大值的和；
//2.第二种就是不包含i位置的值，及在i-1位置取得的最大值。
public class Solution {
    /**
     * @param A: An array of non-negative integers.
     * return: The maximum amount of money you can rob tonight
     */
    public long houseRobber(int[] A) {
        long[] arr = new long[3];
        for (int i = 0; i < A.length; i++) {
            arr[2] = Math.max(arr[0] + A[i], arr[1]);
            arr[0] = arr[1];
            arr[1] = arr[2];
        }
        return arr[2];
    }
}
```

