---
title: LintCode-链表划分
date: '2015-11-06T15:54:02.000Z'
tags: CSDN迁移
---

# LintCode-链表划分

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49684095](https://blog.csdn.net/Sunny_Ran/article/details/49684095)  
容易 链表划分 查看运行结果

29% 通过  
给定一个单链表和数值x，划分链表使得所有小于x的节点排在大于等于x的节点之前。

你应该保留两部分内链表节点原有的相对顺序。

您在真实的面试中是否遇到过这个题？ Yes  
样例  
给定链表 1-&gt;4-&gt;3-&gt;2-&gt;5-&gt;2-&gt;null，并且 x=3

返回 1-&gt;2-&gt;2-&gt;4-&gt;3-&gt;5-&gt;null

这里对题目理解有点问题：

```text
案例一：
输入
1->4->3->2->5->2->null, 3
输出
1->4->2->2->3->5->null
期望答案
1->2->2->4->3->5->null


案列二
输入
1->2->0->3->1->2->1->0->2->2->2->1->0->2->null, 2
输出
0->0->0->1->1->1->1->2->3->2->2->2->2->2->null
期望答案
1->0->1->1->0->1->0->2->3->2->2->2->2->2->null


////不理解他划分好到底需不需要排序，能正确理解的朋友请留言指教。

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
     * @param x: an integer
     * @return: a ListNode 
     */
    public ListNode partition(ListNode head, int x) {
         if(head == null){
             return null;
         }
         ArrayList<Integer> a = new ArrayList<Integer>();
         ArrayList<Integer> b = new ArrayList<Integer>();
         boolean find =false;
        while(head != null){    
            if(head.val == x && !find){
                find =true;
                head = head.next;
                continue;
            }else{
                if(find){
                    if(head.val < x){
                        a.add(head.val);
                    }else{
                        b.add(head.val);
                    }
                }else{
                    a.add(head.val);
                }
                head = head.next;
            }
        }
        ListNode re = new ListNode(0);
        ListNode ree = re;
        for(int n : a){
            re.next = new ListNode(n);
            re = re.next;
        }
    ////    insertionSortList(ree);
        if(find){
            re.next = new ListNode(x);
            re = re.next;
        }
        for(int n : b){
            re.next = new ListNode(n);
            re = re.next;
        }
        return ree.next;
     }

    public static ListNode insertionSortList(ListNode head) {
        boolean flag = true;
        ListNode frist = head;
        ListNode second = head;
        if(head == null || head.next == null){
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

