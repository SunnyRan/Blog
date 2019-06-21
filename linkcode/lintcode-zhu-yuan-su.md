---
title: LintCode-主元素
date: '2015-10-04T21:22:20.000Z'
tags: CSDN迁移
---

# LintCode-主元素

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/48900141](https://blog.csdn.net/Sunny_Ran/article/details/48900141)  
容易 主元素

给定一个整型数组，找出主元素，它在数组中的出现次数严格大于数组元素个数的二分之一。

样例  
给出数组\[1,1,1,1,2,2,2\]，返回 1

挑战  
要求时间复杂度为O\(n\)，空间复杂度为O\(1\)

```text
public class Solution {
    /**
     * @param nums: a list of integers
     * @return: find a  majority number
     */
    public int majorityNumber(ArrayList<Integer> nums) {
            int n=0,k=1;
            n=nums.get(0);
            for(int i=1;i<nums.size();i++){
                if(nums.get(i)==n){
                    k++;
                }else{
                    if(k>1){
                        k--;
                    }else{
                        n=nums.get(++i);
                    }
                }
            }
            return n;
        }
}
```

