---
title: LeetCode-String to Integer (atoi)
date: 2018-07-06 15:06:01
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/80941251   
  Implement atoi which converts a string to an integer.

 The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

 The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

 If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

 If no valid conversion could be performed, a zero value is returned.

 Note:

 Only the space character ’ ’ is considered as whitespace character.   
 Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.   
 Example 1:

 Input: “42”   
 Output: 42   
 Example 2:

 Input: ” -42”   
 Output: -42   
 Explanation: The first non-whitespace character is ‘-‘, which is the minus sign.   
 Then take as many numerical digits as possible, which gets 42.   
 Example 3:

 Input: “4193 with words”   
 Output: 4193   
 Explanation: Conversion stops at digit ‘3’ as the next character is not a numerical digit.   
 Example 4:

 Input: “words and 987”   
 Output: 0   
 Explanation: The first non-whitespace character is ‘w’, which is not a numerical   
 digit or a +/- sign. Therefore no valid conversion could be performed.   
 Example 5:

 Input: “-91283472332”   
 Output: -2147483648   
 Explanation: The number “-91283472332” is out of the range of a 32-bit signed integer.   
 Thefore INT_MIN (−231) is returned.

 
--------
 解题思路：   
 1.去除首尾空格   
 2.找到第一个出现的数字   
 3.找到数字后，后续如果出现字母或者符号后，则结束读取   
 4.每次判断数字的大小是否超出的Integer的范围

 
```
  public static int myAtoi(String str) {
    str = str.trim();
    if (str.isEmpty() || str.length() <= 0) {
      return 0;
    }
    int flag = 1;
    boolean hasFlag = false;
    boolean hasNum = false;
    long re = 0;
    char c;

    for (int i = 0; i < str.length(); i++) {
      c = str.charAt(i);
      if (!isFlag(c) && !isNum(c)) {
        return (int) re * flag;
      }
      if (isFlag(c) && hasNum) {
        return (int) re * flag;
      }
      if (isNum(c)) {
        re = re * 10 + (int) c - 48;
        hasNum = true;
      }
      if (isFlag(c)) {
        if (hasFlag) {
          return (int) re * flag;
        } else {
          flag = 44 - (int) c;
        }
        hasFlag = true;
      }
      // 最大最小值判断
      if (flag == 1 && re > Integer.MAX_VALUE) {
        return Integer.MAX_VALUE;
      }
      if (flag == -1 && -re < Integer.MIN_VALUE) {
        return Integer.MIN_VALUE;
      }
    }
    return (int) re * flag;
  }

  public static boolean isFlag(char c) {
    return c == '+' || c == '-';
  }

  public static boolean isNum(char c) {
    return c >= '0' && c <= '9';
  }
```
   
  