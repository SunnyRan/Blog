---
title: LeetCode-Validate Stack Sequences
date: 2019-01-25 18:49:26
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/86650637   
  Given two sequences pushed and popped with distinct values, return true if and only if this could have been the result of a sequence of push and pop operations on an initially empty stack.

 **Example 1:**

 
```
Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
Output: true
Explanation: We might do the following sequence:
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1

```
 **Example 2:**

 
```
Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
Output: false
Explanation: 1 cannot be popped before 2.

```
 **Note:**

 0 <= pushed.length == popped.length <= 1000  
 0 <= pushed[i], popped[i] < 1000  
 pushed is a permutation of popped.  
 pushed and popped have distinct values.

 解决方案一：

 
```
//时间大概在11ms内完成AC
  public boolean validateStackSequences(int[] pushed, int[] popped) {
    Stack<Integer> stack = new Stack<>();
    int i = 0, j = 0, length = pushed.length;
    while (j < length) {
      if (i < length && popped[j] == pushed[i]) {
        System.out.println("push(" + pushed[i++] + "),pop(" + popped[j++] + "),");
        if (j == length) {
          return true;
        }
      }
      if (!stack.isEmpty() && popped[j] == stack.peek()) {
        System.out.println("pop(" + popped[j++] + "),");
        stack.pop();
        if (j == length) {
          return true;
        }
      } else {
        if (i < length) {
          stack.push(pushed[i]);
          System.out.println("push(" + pushed[i++] + "),");
        } else {
          return false;
        }
      }
    }
    return true;
  }

```
 解决方案二：

 
```
//在讨论区看见的一个方法，就是按照栈的数据进行模拟操作，时间在9ms
public boolean validateStackSequences(int[] pushed, int[] popped) {       
        Stack<Integer> st = new Stack<>();
        int i = 0;
        int j = 0;
        
        while(j<popped.length) {
            if(!st.empty() && st.peek()==popped[j]) {
                st.pop();
                j++;
            } else if(i<popped.length){
                st.push(pushed[i++]);
            } else return false;
        }
        
        return true;
    }

```
 解决方案三：

 
```

//这段代码段AC时间在5ms，我认为是因为用数组的存储格式代替了Stack，优化了时间
public boolean validateStackSequences(int[] pushed, int[] popped) {
        int top, i, j;
        int[] stack;
        if (pushed.length != popped.length)
            return false;
        if (pushed.length == 0)
            return true;
        stack = new int[pushed.length];
        top = -1;
        i = 0;
        j = 0;
        while(true) {
            while((i<pushed.length) && (pushed[i] != popped[j])) {
                stack[++top]=pushed[i++];
            }
            if (i == pushed.length)
                return false;
            i++;
            j++;
            while ((j < popped.length) && (top > -1) && (stack[top] == popped[j])) {
                top--;
                j++;
            }
            if (j == popped.length)
                return true;
        }
    }

```
   
  