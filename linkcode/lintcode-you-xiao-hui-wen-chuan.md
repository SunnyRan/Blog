---
title: LintCode-有效回文串
date: '2015-11-08T22:51:57.000Z'
tags: CSDN迁移
---

# LintCode-有效回文串

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49723759](https://blog.csdn.net/Sunny_Ran/article/details/49723759)  
有效回文串

给定一个字符串，判断其是否为一个回文串。只包含字母和数字，忽略大小写。

样例  
“A man, a plan, a canal: Panama” 是一个回文。

“race a car” 不是一个回文。

注意  
你是否考虑过，字符串有可能是空字符串？这是面试过程中，面试官常常会问的问题。

在这个题目中，我们将空字符串判定为有效回文。

挑战  
O\(n\) 时间复杂度，且不占用额外空间。

```text
public class Solution {
    /**
     * @param s A string
     * @return Whether the string is a valid palindrome
     */
  public boolean isPalindrome(String s) {
        s = s.toLowerCase();
        if(s.length() <= 1){
            return true;
        }
        int a = -1, b = s.length();
        while(b - a > 1){
            while(!cheackData(s.charAt(++a))){
                if(a==s.length()-1){
                    return true;
                }
            }
            while(!cheackData(s.charAt(--b))){
                if(b == 0){
                    return true;
                }
            }
            if(s.charAt(a) != s.charAt(b)) return false;
        }
        return true;
    }

    public boolean cheackData(char c){
        if( c<='z' && c>='a'){
            return true;
        }
        if( c<='9' && c>='0'){
            return true;
        }
        return false;
    }
}
```

