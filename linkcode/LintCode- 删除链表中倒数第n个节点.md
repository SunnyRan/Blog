---
title: LintCode- 删除链表中倒数第n个节点
date: 2015-10-23 15:25:44
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/49362045   
  容易 删除链表中倒数第n个节点 查看运行结果 

 28% 通过   
 给定一个链表，删除链表中倒数第n个节点，返回链表的头节点。

 您在真实的面试中是否遇到过这个题？ Yes   
 样例   
 给出链表1->2->3->4->5->null和 n = 2.

 删除倒数第二个节点之后，这个链表将变成1->2->3->5->null.

 注意   
 链表中的节点个数大于等于n

 挑战   
 O(n)时间复杂度

 
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
     * @param n: An integer.
     * @return: The head of linked list.
     */
    ListNode removeNthFromEnd(ListNode head, int n) {
        if(head.next==null||head==null){
            return null;
        }
        ListNode reHead = head,returnHead = head;
        for(int i=0;i<n;i++){
            head=head.next;
        }
        if(head==null){
            return returnHead.next;
        }
        while(head.next!=null){
            head=head.next;
            reHead=reHead.next;
        }
        reHead.next=reHead.next.next;
        return returnHead;
    }
}


```
   
  