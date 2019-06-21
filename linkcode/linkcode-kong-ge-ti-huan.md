---
title: LinkCode-空格替换
date: '2015-08-30T10:59:24.000Z'
tags: CSDN迁移
---

# LinkCode-空格替换

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/48102849](https://blog.csdn.net/Sunny_Ran/article/details/48102849)  
空格替换

设计一种方法，将一个字符串中的所有空格替换成 %20 。你可以假设该字符串有足够的空间来加入新的字符，且你得到的是“真实的”字符长度。

样例  
对于字符串”Mr John Smith”, 长度为 13

替换空格之后的结果为”Mr%20John%20Smith”

注意  
如果使用 Java 或 Python, 程序中请用字符数组表示字符串。

挑战  
在原字符串\(字符数组\)中完成替换，不适用额外空间

问题分析:  
在原字符串上进行操作,那么就是在检测到’ ‘时,把字符串数组i位置后面的元素右移两位.然后插入”%20”.  
时间复杂杂度O\(n\);空间复杂度O\(1\);

代码:

```text
public class Solution {
    /**
     * @param string: An array of Char
     * @param length: The true length of the string
     * @return: The true length of new string
     */
       public int replaceBlank(char[] string, int length) {
        int k=0;
        for (int i = 0; i < length+k; i++) {
            if(string[i]==' '){
                for(int j= length+k-1;j>i;j--){
                    string[j+2]=string[j];
                }
                k=k+2;
                string[i]='%';
                string[i+1]='2';
                string[i+2]='0';

            }
        }
        return length+k;
    }
}
```

