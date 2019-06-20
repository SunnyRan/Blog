---
title: LintCode-删除链表中的元素
date: 2016-02-26 19:29:20
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/50752091   
  删除链表中的元素   
 删除链表中等于给定值val的所有节点。

 样例   
 给出链表 1->2->3->3->4->5->3, 和 val = 3, 你需要返回删除3之后的链表：1->2->4->5。

 
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    /**
     * @param head a ListNode
     * @param val an integer
     * @return a ListNode
     */
    public ListNode removeElements(ListNode head, int val) {
        ListNode result = head;
        if(head==null) return null;
        while (head.next!=null)
        {
            if(head.next.val == val)
            {
                if(head.next.next!=null)
                    head.next=head.next.next;
                else 
                {
                  head.next=null;
                  break;
                }
            }
            else
            {
                head=head.next;
            }

        }
        if(result.val==val) return result.next;
        return result;
    }
}
```
   
  