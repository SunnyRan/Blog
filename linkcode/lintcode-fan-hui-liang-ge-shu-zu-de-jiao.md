---
title: LintCode-返回两个数组的交
date: '2016-08-09T00:27:43.000Z'
tags: CSDN迁移
---

# LintCode-返回两个数组的交

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/52157379](https://blog.csdn.net/Sunny_Ran/article/details/52157379)  
返回两个数组的交

样例  
nums1 = \[1, 2, 2, 1\], nums2 = \[2, 2\], 返回 \[2\].

没什么难度，用Hash表即可

```text
public class Solution {
    /**
     * @param nums1 an integer array
     * @param nums2 an integer array
     * @return an integer array
     */

    public int[] intersection(int[] nums1, int[] nums2) {
            HashSet<Integer> set =new HashSet<Integer>();
            for (int i : nums2) {
                set.add(i);
            }
            HashSet<Integer> re = new HashSet<Integer>();
            for (int i : nums1) {   
                if(set.contains(i))
                {
                    re.add(i);
                }
            }
            Iterator<Integer> iterator=re.iterator();
            int[] reInt = new int[re.size()];
            int i =0;
            while(iterator.hasNext()){
                reInt[i++] = iterator.next();
            }
            return reInt;
        }
}
```

