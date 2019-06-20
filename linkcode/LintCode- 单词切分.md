---
title: LintCode- 单词切分
date: 2016-03-09 19:13:48
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/50837837   
  单词切分 

 给出一个字符串s和一个词典，判断字符串s是否可以被空格切分成一个或多个出现在字典中的单词。

 样例   
 s = “lintcode”

 dict = [“lint”,”code”]

 返回 true 因为”lintcode”可以被空格切分成”lint code”

 
```
//Memory Limit Exceeded
//这是一个递归版本的
//原理为从第一个s[i]位置开始遍历，查找s.substring(0.i)是否包含于字典dict内。 一直到i==dict中单词最长的长度。
//如果没有找不到，则返回false，如果找到了，则在s[i+1]的位置重复上述过程。直到i==s.length();

public class Solution {
    /**
     * @param s: A string s
     * @param dict: A dictionary of words dict
     */
 public static boolean flag=false;
    public boolean wordBreak(String s, Set<String> dict) {
        if (s.length() < 1 && dict.size() < 1)
            return true;
        if (s == null || s.length() < 1 || dict == null || dict.size() < 1) {
            return false;
        }
        int max = 0;
        for (String a : dict) {
            if (a.length() > max) {
                max = a.length();
            }
        }
        this.find(s, dict, max);
        return flag;
    }

    public void find(String s, Set<String> dict, int max) {
        for (int i = 1; i<=s.length()&&i<=max; i++) {
            if(dict.contains(s.substring(0,i))){
                if(i==s.length()) {
                    flag=true;
                }
                else{
                    String str = new String(s.substring(i));
                    this.find(str, dict, max);
                }
            }
        }
    }
}
```
   
  