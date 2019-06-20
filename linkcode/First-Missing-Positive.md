---
title: LeetCode-First Missing Positive
date: 2018-06-06 16:35:06
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/80597135   
  Given an unsorted integer array, find the smallest missing positive integer.

 **Example 1:**

 Input: [1,2,0]   
 Output: 3   
 **Example 2:**

 Input: [3,4,-1,1]   
 Output: 2   
 **Example 3:**

 Input: [7,8,9,11,12]   
 Output: 1   
 Note:

 Your algorithm should run in O(n) time and uses constant extra space.

 **题目解析：**   
 找到第一个不存在的最小正整数。

 关键点： [第一个] [最小的正整数]

 [第一个]：因为忽略了这一层，所以采用异或遍历最大最小值找出确实的‘唯一’数的方法失效。   
 [最小的正整数]：这里面有一个规则就是不为零，若大于0的数全部存在，则返回最大值MAX+1。

 **思路1**   
 这里给出一种时间复杂的O(n) 空间复杂度O(1)的算法。   
 由上述分析可知，确实的数一定是正数，并且原有数组中可能存在负数，或者重复值。   
 所以整个原数组最大也只会有数组Length个连续的正整数。（正好可以按照顺序排序到原有数组秩为N-1的位置上）   
 借鉴桶排序的思想，我们把Array[i]位置上的数X放在Array[X-1]的位置上(如果X不在数组的长度的范围内，则跳过)   
 举例说明：   
 [3,1,4,5,2,0,-1]   
 变化后为：   
 [1,2,3,4,5,0,-1]

 遍历变化后的数组，第一个不符合Array[i] == i+1的数就是我们要找数。

 
--------
 
```
    public static int firstMissingPositive(int[] nums) {
        int len = nums.length,k = 0;
        //循环交换对应秩的数组的内容
        while(k<len){ 
        //如果nums[k]的值满足在数组的秩序Length+1的范围内，并且没有重复元素，则进行交换
            if(nums[k]>0 && nums[k]<len+1 && nums[k]!=k+1 && nums[k] != nums[nums[k]-1]){
                int x = nums[nums[k]-1];
                nums[nums[k]-1] = nums[k];
                nums[k] = x;
            }
            //若不满足上述条件，检索下一个数
            else
            {
                k++;
            }
        }
        //循环数组，找出缺数的数
        for (k = 0; k < len; k++) {
            if (nums[k] != k+1)
                return k+1;
        }
        return k+1;
    }
```
   
  