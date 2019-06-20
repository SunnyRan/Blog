---
title: LeetCode-Missing Number
date: 2017-12-14 17:31:43
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/78805215   
  ## Description

 Given an array containing n distinct numbers taken from 0, 1, 2, …, n, find the one that is missing from the array.

 Example 1

 
```
Input: [3,0,1]
Output: 2
Example 2

Input: [9,6,4,2,3,5,7,0,1]
Output: 8
```
 Note:   
 Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

 
## Solution

 **思路1：**   
 因为数组是连续的，所以可以运用加法进行求和后得到SUM1 ，然后遍历数组后求和得到SUM2,缺失的数即为SUM1-SUM2。   
 时间复杂度O(n),空间复杂读O(1)。   
 缺点：如果数组过长，可能超过Integer.MAX

 **思路2**   
 利用排序的思想，把对应的数移动到对应的下标处。比如A[i] = K;则将A[i] 与A[k]进行交换。直到数组基本有序（因为缺失一个数）。   
 1.最后一个数无法跟下标对应，[3,0,1] => [0,1,3] 因为A[2] != 2所以缺失的数是2.   
 2.所有数都跟下标对应[2,1,0,3] => [0,1,2,3] 则缺失的数是4

 时间复杂度O(n),空间复杂读O(1)。   
 缺点:算法较为复杂。需要多次交换数组元素位置，费时。

 **思路3**   
 使用位操作Bit Manipulation来解的，用到了异或操作的特性，相似的题目有Single Number 单独的数字, Single Number II 单独的数字之二和Single Number III 单独的数字之三。那么思路是既然0到n之间少了一个数，我们将这个少了一个数的数组合0到n之间完整的数组异或一下，那么相同的数字都变为0了，剩下的就是少了的那个数字了.

 
```
//时间复杂度O(n)
//空间复杂度O(1)
    public int missingNumber(int[] nums) {
        int k = nums.length;
        for(int i = 0;i< nums.length;i++)
        {
            k^=nums[i]^i;
        }
        return k;
    }
```
   
  