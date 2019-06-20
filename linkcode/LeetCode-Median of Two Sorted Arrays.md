---
title: LeetCode-Median of Two Sorted Arrays
date: 2017-02-28 20:31:18
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/58644001   
  **4. Median of Two Sorted Arrays**

 There are two sorted arrays nums1 and nums2 of size m and n respectively.

 Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

 Example 1:   
 nums1 = [1, 3]   
 nums2 = [2]

 The median is 2.0   
 Example 2:   
 nums1 = [1, 2]   
 nums2 = [3, 4]

 The median is (2 + 3)/2 = 2.5

 **思路：**   
 用两个指针*A、*B指向两个数组，因为两个数组已经排序，所以*A、*B指针所对应的数据的排序就是合并后数据的排序，根据两个数组的长度即可确定中位数的位置X。当指针*A、*B移动位置的合为X时，该数就是中位数。   
 时间复杂度O(n),空间复杂度:O(1).

 
```
/**
 * Created by sunny.su on 2017/2/28.
 */
public class MedianOfTwoSortedArrays {
    public static void main(String[] args) {
        int[] nums1 = {};
        int[] nums2 = {2,3};
        System.out.println(findMedianSortedArrays(nums1, nums2));
    }

    public static double findMedianSortedArrays(int[] nums1, int[] nums2) {
        double re = 0;
        int total = nums1.length + nums2.length, x = nums1.length, y = nums2.length;
        if (total == 2) {
            if (x > 0 && y > 0) {
                return (nums1[--x] + nums2[--y]) / 2.0;
            }
            return x > 0 ? (nums1[--x] + nums1[--x]) / 2.0 : (nums2[--y] + nums2[--y]) / 2.0;
        }
        if (total == 1) {
            return x > 0 ? nums1[--x] : nums2[--y];
        }
        while (x + y > Math.ceil(total / 2)) {
            if (x == 0) re = nums2[--y];
            if (y == 0) re = nums1[--x];
            if (x > 0 && y > 0) re = nums1[x - 1] > nums2[y - 1] ? nums1[--x] : nums2[--y];
        }
        if (total % 2 == 0) {
            if (x == 0) re += nums2[--y];
            if (y == 0) re += nums1[--x];
            if (x > 0 && y > 0) re += nums1[x - 1] > nums2[y - 1] ? nums1[--x] : nums2[--y];
            re = re / 2;
        }
        return re;
    }
}




```
   
  