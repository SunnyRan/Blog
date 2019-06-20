---
title: LintCode-链表插入排序
date: 2015-11-06 11:50:12
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/49681039   
   #### 容易 链表插入排序  


   
 用插入排序对链表排序

   
 您在真实的面试中是否遇到过这个题？ Yes  
   
 样例 Given  `1->3->2->0->null` , return  `0->1->2->3->null` 

   
   
   
 冒泡法遍历  
   


 

 

 


```
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @return: The head of linked list.
     */
        public ListNode insertionSortList(ListNode head) {
		boolean flag = true;
		ListNode frist = head;
		ListNode second = head;
		if(head.next == null || head == null){
			return head;
		}else{
			frist = head.next;
		}
		while(flag){
			flag = false;
			do{
				if(frist.val<second.val){
					int x =frist.val;
					frist.val = second.val;
					second.val = x;
					flag = true;
				}			
				frist = frist.next;
				second = second.next;
			}while(frist != null);
			frist = head.next;
			second = head;
		}
		return head;
	}
}
```
  
  
   
 