# Even Linked List



## Description

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O\(1\) space complexity and O\(nodes\) time complexity.

### Example:

Given 1-&gt;2-&gt;3-&gt;4-&gt;5-&gt;NULL,  
return 1-&gt;3-&gt;5-&gt;2-&gt;4-&gt;NULL.

### Note:

The relative order inside both the even and odd groups should remain as it was in the input.  
The first node is considered odd, the second node even and so on …

## Solution

这道题给了我们一个链表，让我们分开奇偶节点，所有奇节点在前，偶节点在后。我们可以使用两个链表来做，prevOdd指向奇节点，prevEven指向偶节点  
1.让prevOdd.next指向prevEven后面的那个奇节点。  
2.prevOdd指向他的下一个节点。  
3.让prevEven.next指向prevOdd后面的那个偶节点  
4.prevEven指向他的下一个节点。  
以上循环执行。

Final 奇节点prevOdd.next指向偶节点prevEven的根节点evenHead  
参见代码如下：

```text
public class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null)
            return head;
        ListNode oddHead = head,evenHead =head.next;
        ListNode prevOdd = oddHead,prevEven = evenHead;

        while(prevOdd.next != null && prevEven.next != null){
            prevOdd.next = prevEven.next;
            prevOdd = prevOdd.next;

            prevEven.next = prevOdd.next;
            prevEven = prevEven.next;
        }
        prevOdd.next = evenHead;

        return oddHead;
    }
}
```

一开始理解题目错误  
以为是让把链表的奇数偶数排序。  
这里给出错误理解下的正确代码：

```text
    public ListNode oddEvenList(ListNode head) {
        ListNode head0 = new ListNode(0),head00 = head0;
        ListNode head1 = new ListNode(0),head11 = head1;
        int k = head ==null? 1: head.val%2;// 判断第一个数奇偶
        while (head !=null)
        {
            if(head.val%2 == k)//奇数
            {
                head1.next = head;
                head1 = head1.next;
            }
            else //偶数
            {
                head0.next = head;
                head0 = head0.next;
            }
            head = head.next;
        }
        head0.next = null; //很重要的一个语句 ！！！ 去除可能出现环链  测试数据 1->2->3;
        head1.next = head00.next;
        return  head11.next;
    }
```

