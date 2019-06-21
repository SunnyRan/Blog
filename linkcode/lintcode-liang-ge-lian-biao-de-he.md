---
title: LintCode-两个链表的和
date: '2015-10-04T19:50:33.000Z'
tags: CSDN迁移
---

# LintCode-两个链表的和

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/48898893](https://blog.csdn.net/Sunny_Ran/article/details/48898893)

## 容易 两个链表的和

你有两个用链表代表的整数，其中每个节点包含一个数字。数字存储按照在原来整数中相反的顺序，使得第一个数字位于链表的开头。写出一个函数将两个整数相加，用链表形式返回和。

样例 给出两个链表 `3->1->5->null` 和 `5->9->2->null` ，返回 `8->0->8->null`

```text
/**
 * Definition for singly-linked list.
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
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
  public ListNode addLists(ListNode l1, ListNode l2) {
        ListNode l3=new ListNode(0);
        ListNode l =l3;
        boolean run=true;
        int flag=0;
        do{
            int k=l1.val+l2.val+flag;
            flag=(int)k/10;
            k=k%10;
            l3=l3.next=new ListNode(k);

            if(l1.next==null&&l2.next==null){
                run=false;
                if(flag!=0){
                    l3.next=new ListNode(flag);
                }
            }
            if(l1.next!=null){
                l1=l1.next;
            }else{
                l1.val=0;
            }
            if(l2.next!=null){
                l2=l2.next;
            }else{
                l2.val=0;
            }
        }while(run);

        return l.next;

    }
}
```

一个很简单的遍历。

