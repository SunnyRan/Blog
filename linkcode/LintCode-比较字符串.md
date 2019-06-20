---
title: LintCode-比较字符串
date: 2015-11-08 22:08:18
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/49722475   
  比较字符串 

 比较两个字符串A和B，确定A中是否包含B中所有的字符。字符串A和B中的字符都是 大写字母

 样例   
 给出 A = “ABCD” B = “ACD”，返回 true

 给出 A = “ABCD” B = “AABC”， 返回 false

 注意   
 在 A 中出现的 B 字符串里的字符不需要连续或者有序。

 
```
public class Solution {
    /**
     * @param A : A string includes Upper Case letters
     * @param B : A string includes Upper Case letter
     * @return :  if string A contains all of the characters in B return true else return false
     */
    public boolean compareStrings(String A, String B) {
             int [] re = new int [26];
             for(int i = 0; i < A.length(); i++){
                 re[(int)(A.charAt(i)-'A')]++;
             }
             for(int i = 0; i<B.length(); i++){
                 if(re[(int)(B.charAt(i)-'A')]-- == 0){
                     return false;
                 }
             }
             return true;
        }
}
```
   
  