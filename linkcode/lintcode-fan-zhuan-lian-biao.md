---
title: LintCode-翻转链表
date: '2015-11-10T20:28:38.000Z'
tags: CSDN迁移
---

# LintCode-翻转链表

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49764775](https://blog.csdn.net/Sunny_Ran/article/details/49764775)

## 翻转链表

翻转一个链表

样例 给出一个链表1-&gt;2-&gt;3-&gt;null，这个翻转后的链表为3-&gt;2-&gt;1-&gt;null

挑战 在原地一次翻转完成

通过三个链表指向，依次轮转。完成原地一次翻转。

```text
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
     * @param head: The head of linked list.
     * @return: The new head of reversed linked list.
     */
    public ListNode reverse(ListNode head) {
         ListNode change = null,save = null;
         while(head != null){
             save = head.next;
             head.next = change;
             change = head;
             head = save;
             if(save == null){
                 break;
             }
         }
         return change;
        }
}
```

