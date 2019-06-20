---
title: LintCode-最大间距
date: 2016-03-07 16:37:52
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/50820413   
  最大间距 

 给定一个未经排序的数组，请找出其排序表中连续两个要素的最大间距。

 如果数组中的要素少于 2 个，请返回 0.

 可以假定数组中的所有要素都是非负整数，且最大不超过 32 位整数。

 样例

 给定数组 [1, 9, 2, 5]，其排序表为 [1, 2, 5, 9]，其最大的间距是在 5 和 9 之间，= 4.

 
```
//第一次用的冒泡法排序后 出现超时。
class Solution {
    /**
     * @param nums: an array of integers
     * @return: the maximum difference
     */
    public int maximumGap(int[] nums) {
        this.sort(nums);
        int max = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] - nums[i+1] >= max) {
                max = nums[i] - nums[i+1];
            }
        }
        return max;
    }

    public void sort(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            boolean flag = false;
            for(int k =0;k<nums.length-i;k++){          
                if (nums[k + 1] > nums[k]) {
                    int a = nums[k];
                    nums[k] = nums[k + 1];
                    nums[k + 1] = a;
                    flag = true;
                }
            }
            if (!flag)  
                return ;
        }
    }

}
```
 ===========================================================================

 
```
//第二次使用快排后，出现内存溢出
class Solution {
    /**
     * @param nums: an array of integers
     * @return: the maximum difference
     */
    public int maximumGap(int[] nums) {
        this.quickSort(nums, 0, nums.length - 1);
        int max = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i + 1] - nums[i] >= max) {
                max = nums[i + 1] - nums[i];
            }
        }
        return max;
    }

    public void quickSort(int[] nums, int left, int right) {
        int first = nums[left];
        int i = left;
        int j = right;
        if (left == right)
            return;
        while (left < right) {
            if (nums[left] > first) {
                this.change(nums, left, right);
                right--;
            } else {
                left++;
            }
        }
        if (nums[left] <= first) {
            this.change(nums, i, left);
        } else {
            this.change(nums, i, --left);
        }
        if (left - 1 > i)
            this.quickSort(nums, i, left - 1);
        if (left + 1 < j)
            this.quickSort(nums, left + 1, j);
    }

    public void change(int[] nums, int left, int right) {
        int a = nums[right];
        nums[right] = nums[left];
        nums[left] = a;
    }
}
```
 ===========================================================================

 
```
//第三次改为希尔排序后 成功AC
class Solution {
    /**
     * @param nums: an array of integers
     * @return: the maximum difference
     */
    public int maximumGap(int[] nums) {
        this.shellSort(nums);
        int max = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i + 1] - nums[i] >= max) {
                max = nums[i + 1] - nums[i];
            }
        }
        return max;
    }

    public void shellSort(int[] array)
    {
        int increment = array.length;
        do
        {
            increment = increment / 2;  // 增量序列
            for (int i = increment; i < array.length; i++)
            {
                if (array[i] < array[i - increment])
                {
                    int guard = array[i];
                    int j;
                    for (j = i - increment; (j >= 0) && (guard < array[j]); j -= increment)     //记录后移，查找插入位置
                    {
                        array[j + increment] = array[j];
                    }
                    array[j + increment] = guard;  // 插入
                }
            }
        } while (increment > 1);
    }
}
```
   
  