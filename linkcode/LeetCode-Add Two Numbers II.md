---
title: LeetCode-Add Two Numbers II
date: 2017-07-30 20:03:56
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/76404417   
  # Description

 You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

 You may assume the two numbers do not contain any leading zero, except the number 0 itself.

 
# Follow up:

 What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

 
# Example:

 Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)   
 Output: 7 -> 8 -> 0 -> 7

 
# Solution

 思路：   
 这里使用了两个栈将两个链表的值反置；然后一次按位相加即可得到和。   
 这里有几个点需要注意下：   
 1. 相加后得到的数据是从低位开始的。需要进行反置(或者直接在返回值上做文章)   
 2. 按位相加得到和后，需要注意下最后是否还需要高位进一；如果需要则需要补充一个高位。

 
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack<Integer> sta1 = new Stack<Integer>();
        Stack<Integer> sta2 = new Stack<Integer>();
        while (l1 != null) {
            sta1.push(l1.val);
            l1 = l1.next;
        }
        while (l2 != null) {
            sta2.push(l2.val);
            l2 = l2.next;
        }

        ListNode re  = null;
        int x = 0;
        while (!sta1.isEmpty() || !sta2.isEmpty()) {
            x += (sta1.isEmpty() ? 0 : sta1.pop()) + (sta2.isEmpty() ? 0 : sta2.pop());
            ListNode node = new ListNode(x%10);
            node.next = re;
            re = node;
            x = x / 10;
        }
        if(x ==1){
            ListNode node = new ListNode(1);
            node.next = re;
            re = node;
        }
        return re;
    }
}
```
   
  