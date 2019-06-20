---
title: LintCode-删除排序链表中的重复元素
date: 2015-10-23 15:05:43
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/49361675   
  容易 删除排序链表中的重复元素 查看运行结果 

 38% 通过   
 给定一个排序链表，删除所有重复的元素每个元素只留下一个。

 您在真实的面试中是否遇到过这个题？ Yes   
 样例   
 给出1->1->2->null，返回 1->2->null

 给出1->1->2->3->3->null，返回 1->2->3->null

 
```
/**
 * Definition for ListNode
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param ListNode head is the head of the linked list
     * @return: ListNode head of linked list
     */
    public static ListNode deleteDuplicates(ListNode head) { 
        Set<Integer> s = new HashSet<Integer>();
        ListNode oldHead = head;
        if(head==null){
            return null;
        }
        s.add(head.val);
        do{
            if(!s.contains(head.next.val)){
                s.add(head.next.val);
                head=head.next;
            }else{
                head.next=head.next.next;
            }
        }while(head.next!=null);
        return oldHead;

    }  
}

```
   
  