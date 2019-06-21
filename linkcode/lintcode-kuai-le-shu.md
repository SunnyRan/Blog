---
title: LintCode-快乐数
date: '2016-02-26T19:52:47.000Z'
tags: CSDN迁移
---

# LintCode-快乐数

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/50752178](https://blog.csdn.net/Sunny_Ran/article/details/50752178)  
快乐数  
写一个算法来判断一个数是不是”快乐数”。

一个数是不是快乐是这么定义的：对于一个正整数，每一次将该数替换为他每个位置上的数字的平方和，然后重复这个过程直到这个数变为1，或是无限循环但始终变不到1。如果可以变为1，那么这个数就是快乐数。

样例  
19 就是一个快乐数。

1^2 + 9^2 = 82  
8^2 + 2^2 = 68  
6^2 + 8^2 = 100  
1^2 + 0^2 + 0^2 = 1

```text
public class Solution {
    /**
     * @param n an integer
     * @return true if this is a happy number or false
     */
  public boolean isHappy(int n) {
        boolean flag = false;
        long count = n;
        int limit =0;
        while(count!=1&&limit<1000)
        {
            count=this.calculate(count);
            if(count ==n) break;
            limit++;
        }
        if(count==1) flag=true;
        return flag;

    }

    public long calculate(long count) {
        long total = 0;
        long a = 0;
        while (count != 0) {
            a = count % 10;
            count = count / 10;
            total = total + a * a;
        }
        return total;
    }
}
```

