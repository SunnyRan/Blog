---
title: LintCode- 恢复IP地址
date: '2016-02-28T15:45:31.000Z'
tags: CSDN迁移
---

# LintCode- 恢复IP地址

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/50760284](https://blog.csdn.net/Sunny_Ran/article/details/50760284)  
恢复IP地址  
给一个由数字组成的字符串。求出其可能恢复为的所有IP地址。  
样例

给出字符串 “25525511135”，所有可能的IP地址为：

\[  
“255.255.11.135”,  
“255.255.111.35”  
\]  
（顺序无关紧要）

```text
public class Solution {
    /**
     * @param s the IP string
     * @return All possible valid IP addresses
     */
public ArrayList<String> restoreIpAddresses(String s) {
        ArrayList<String> result = new ArrayList<String>();
        String str = new String();
        this.findPaths(s, str, s.length(), 0, 4, result);
        return result;
    }

    public void findPaths(String s, String re, int n, int index, int finish,
            ArrayList<String> result) {
        if (index == n && finish == 0) {
            result.add(re);
        }
        if ((finish == 0 && index < n) || (index == n && finish > 0))
            return;
        int a = 0;
        String s0 = new String();
        s0 = re;
        boolean flag =true;
        while (index < n&&flag) {
            if(a==0&&s.charAt(index)=='0'){
                flag=false;
            }
            a = a * 10 + s.charAt(index++) - '0';
            if (a >= 0 && a < 256) {
                String s1 = new String();
                if (finish == 1)
                    s1 = s0 + Integer.toString(a);
                else
                    s1 = s0 + Integer.toString(a) + ".";

                this.findPaths(s, s1, n, index, finish - 1, result);
            }
        }
    }
}
```

