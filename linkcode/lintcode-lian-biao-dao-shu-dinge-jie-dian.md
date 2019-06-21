---
title: LintCode- 链表倒数第n个节点
date: '2015-11-09T00:04:05.000Z'
tags: CSDN迁移
---

# LintCode- 链表倒数第n个节点

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49725921](https://blog.csdn.net/Sunny_Ran/article/details/49725921)  
链表倒数第n个节点

找到单链表倒数第n个节点，保证链表中节点的最少数量为n。

样例  
给出链表 3-&gt;2-&gt;1-&gt;5-&gt;null和n = 2，返回倒数第二个节点的值1.

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
     * @param head: The first node of linked list.
     * @param n: An integer.
     * @return: Nth to last node of a singly linked list. 
     */
    ListNode nthToLast(ListNode head, int n) {
            int count = 0;
            ListNode re = head;
            while(head != null){
                count++;
                head = head.next;
            }
            count = count - n;
            while(count-- > 0){
                re =re.next;
            }
            return re;
        }
}
```

