---
title: LintCode-生成括号
date: '2016-02-27T23:29:07.000Z'
tags: CSDN迁移
---

# LintCode-生成括号

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/50757561](https://blog.csdn.net/Sunny_Ran/article/details/50757561)  
生成括号

给定 n 对括号，请写一个函数以将其生成新的括号组合，并返回所有组合结果。

样例  
给定 n = 3, 可生成的组合如下:

“\(\(\(\)\)\)”, “\(\(\)\(\)\)”, “\(\(\)\)\(\)”, “\(\)\(\(\)\)”, “\(\)\(\)\(\)”

```text
public class Solution {
    /**
     * @param n n pairs
     * @return All combinations of well-formed parentheses
     */
public ArrayList<String> generateParenthesis(int n) {
        ArrayList<String> result = new ArrayList<String>();
        String str = new String();
        findPaths(0, 0, n, str, result);
        return result;
    }

    public void findPaths(int n, int m, int sum, String str, List<String> result) {
        String str0 = new String();
        String str1 = new String();
        if (sum - n > 0) {
            str0 = str + "(";
            this.findPaths(n + 1, m + 1, sum, str0, result);
        }
        if (m > 0) {
            str1 = str + ")";
            this.findPaths(n, m - 1, sum, str1, result);
        }
        if(sum==n&&m==0) result.add(str);
    }
}
```

