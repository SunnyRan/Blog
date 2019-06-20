---
title: LintCode-删除排序数组中的重复数字 II
date: 2015-10-23 12:51:46
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/49359961   
   #### 容易 删除排序数组中的重复数字 II 查看运行结果 

   
   
 30% 通过  
   
 跟进“删除重复数字”：

 如果可以允许出现两次重复将如何处理？

 

   
 您在真实的面试中是否遇到过这个题？ Yes  
   
 样例 给出数组A =[1,1,1,2,2,3]，你的函数应该返回长度5，此时A=[1,1,2,2,3]。

   
   
   
   


 

 

 
```
public class Solution {
    /**
     * @param A: a array of integers
     * @return : return an integer
     */
    public int removeDuplicates(int[] nums) {
		HashMap<Integer , Integer> map = new HashMap<Integer , Integer>(); 

		int n=0;
		for(int i=0;i<nums.length;i++){
			if(map.get(nums[i])==null){
					map.put(nums[i], 1);
					nums[n++]=nums[i];
			}else{
				if(map.get(nums[i])<2){
					map.put(nums[i], map.get(nums[i])+1);
					nums[n++]=nums[i];
				}
			}
		}
		return n;
    }
}
```
  
  


   
 