---
title: LintCode- 合并区间
date: 2016-03-07 17:44:13
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/50821070   
  合并区间 

 给出若干闭合区间，合并所有重叠的部分。

 样例

 给出的区间列表 => 合并后的区间列表：

 [ [   
 [1, 3], [1, 6],   
 [2, 6], => [8, 10],   
 [8, 10], [15, 18]   
 [15, 18] ]   
 ]

 
```
//先排序 然后根据i位置的end与i+1位置的star比较 如果有重叠 则表示可以合并 合并后的end为i与i+1位置上的最大值。
/**
 * Definition of Interval:
 * public class Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */

class Solution {
    /**
     * @param intervals: Sorted interval list.
     * @return: A new sorted interval list.
     */
    public List<Interval> merge(List<Interval> intervals) {
        this.sort(intervals);
        for (int i=0;i<intervals.size()-1;i++) {
            Interval data = intervals.get(i);
            if(data.end>=intervals.get(i+1).start){
                data.end=intervals.get(i+1).end>data.end?intervals.get(i+1).end:data.end;
                intervals.remove(i+1);
                i--;
            }
        }
        return intervals;
    }

    public void sort(List<Interval> intervals){
        for (int i = intervals.size() - 1; i > 0; --i)
        {
            for (int j = 0; j < i; ++j)
            {
                if(intervals.get(j).start>intervals.get(j+1).start){
                    Interval old = new Interval(intervals.get(j).start,intervals.get(j).end);
                    intervals.get(j).end=intervals.get(j+1).end;
                    intervals.get(j).start=intervals.get(j+1).start;
                    intervals.get(j+1).end=old.end;
                    intervals.get(j+1).start=old.start;
                }
            }
        }
    }
}
```
   
  