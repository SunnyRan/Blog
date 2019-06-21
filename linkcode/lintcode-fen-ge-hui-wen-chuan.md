---
title: LintCode- 分割回文串
date: '2016-03-11T23:14:28.000Z'
tags: CSDN迁移
---

# LintCode- 分割回文串

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/50859671](https://blog.csdn.net/Sunny_Ran/article/details/50859671)  
分割回文串

给定一个字符串s，将s分割成一些子串，使每个子串都是回文串。

返回s所有可能的回文串分割方案。

样例

给出 s = “aab”，返回

\[  
\[“aa”, “b”\],  
\[“a”, “a”, “b”\]  
\]

```text
//这里运用了回溯算法 递推调用 动态规划的算法思想
public class Solution {
    /**
     * @param s: A string
     * @return: A list of lists of string
     */
    public List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<List<String>>();
        List<String> re = new ArrayList<String>();
        this.find(s, re, 0, result);
        return result;
    }

//用来遍历
//思路为从s[0]位置开始向后寻找可分割的位置，寻找函数为isRight（用来检测传入的字符串是否为回文）
//例如：{[a],b,a,c} 遍历到第一个位置时 出现回文 则递归调用find函数 并且把标记位置star指向第二个位置
//递归进入的函数如图：{a,[b],a,c}
//下面说下第二层循环(即j==1时的情况) 此时isRight函数检测的字符串为{[a,b],a,c} 不是回文 所以j++
//进入第三层循环（即j==2时的情况） 此时isRight函数检测的字符串为{[a,b,a],c}是回文 递归调用。
    public void find(String s, List<String> result, int star,
            List<List<String>> re) {
        int len = s.length();
        if (star == len) {
            re.add(result);
            return;
        }

        for (int j = star; j < len; j++) {
            if (isRight(s.substring(star, j + 1))) {
                List<String> add = new ArrayList<String>();
                add.addAll(result);
                add.add(s.substring(star, j + 1));
                this.find(s, add, j + 1, re);

            }
        }
    }

//检测输入字符串是否为回文
    public boolean isRight(String s) {
        int len = s.length();
        for (int i = 0; i < len / 2; i++) {
            if (s.charAt(i) != s.charAt(len - i - 1)) {
                return false;
            }
        }
        return true;
    }

}
```

