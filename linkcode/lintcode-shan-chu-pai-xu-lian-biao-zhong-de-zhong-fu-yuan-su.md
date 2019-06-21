---
title: LintCode-删除排序链表中的重复元素
date: '2015-10-23T15:05:43.000Z'
tags: CSDN迁移
---

# LintCode-删除排序链表中的重复元素

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49361675](https://blog.csdn.net/Sunny_Ran/article/details/49361675)  
容易 删除排序链表中的重复元素 查看运行结果

38% 通过  
给定一个排序链表，删除所有重复的元素每个元素只留下一个。

您在真实的面试中是否遇到过这个题？ Yes  
样例  
给出1-&gt;1-&gt;2-&gt;null，返回 1-&gt;2-&gt;null

给出1-&gt;1-&gt;2-&gt;3-&gt;3-&gt;null，返回 1-&gt;2-&gt;3-&gt;null

```text
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

