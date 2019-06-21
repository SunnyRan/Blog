---
title: LintCode- 翻转字符串
date: '2015-11-10T21:53:50.000Z'
tags: CSDN迁移
---

# LintCode- 翻转字符串

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49765927](https://blog.csdn.net/Sunny_Ran/article/details/49765927)  
翻转字符串

给定一个字符串，逐个翻转字符串中的每个单词。

样例  
给出s = “the sky is blue”，返回”blue is sky the”

说明  
单词的构成：无空格字母构成一个单词  
输入字符串是否包括前导或者尾随空格？可以包括，但是反转后的字符不能包括  
如何处理两个单词间的多个空格？在反转字符串中间空格减少到只含一个

```text
//方法一
public class Solution {
    /**
     * @param s : A string
     * @return : A string
     */
    public String reverseWords(String s) {
        ArrayList<String> arr = new ArrayList<String>();
        if(s==""){
            return "";
        }
        for(int i = 0; i<s.length();i++ ){
            while(i<s.length()&&s.charAt(i)==' '){
                i++;
            }
            int a = i;
            while(i<s.length()&&s.charAt(i)!=' '){
                i++;
            }
            int b = i;
            if(i==s.length()-1){
                arr.add(s.substring(a));
                break;
            }
            arr.add(s.substring(a, b));
        }
        String re = "";
        Collections.reverse(arr);
        for(String x : arr){
            re = re + " "+x;
        }
        return re.substring(1).trim();
    }
}





//方法二

public class Solution {
    /**
     * @param s : A string
     * @return : A string
     */
    public String reverseWords(String s) {
        String [] a = s.split(" ");
        String re ="";
        for(int i =a.length-1;i>-1;i--){
            if(i==0){
                re = re+a[i];
            }else{
                re = re + a[i] + " ";
            }
        }
        return re;
    }
}
```

