---
title: LinkCode-最多有多少个点在一条直线上
date: 2015-08-30 10:38:11
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/48102521   
  最多有多少个点在一条直线上

 给出二维平面上的n个点，求最多有多少点在同一条直线上。

 样例   
 给出4个点：(1, 2), (3, 6), (0, 0), (1, 3)。

 一条直线上的点最多有3个。

 问题分析:   
 可以根据A点跟其他点所构成线段的斜率判断与A点共线的点的个数.遍历所有点,取出最大值即可,   
 1.考虑重复点的情况.   
 2.考虑斜率为无穷大的情况.

 
```
import java.util.HashMap;

//定义坐标点

class Point {
    int x;
    int y;
    Point() { x = 0; y = 0; }
    Point(int a, int b) { x = a; y = b; }
}

public class Solution {
//判断坐标点集是否为空
    public int maxPoints(Point[] points) {
        if(points==null||points.length==0){
            return 0;
        }
//建立Hash表
        HashMap<Double,Integer> map= new HashMap<Double,Integer>();
        int max=1;
        for(int i=0;i<points.length;i++){
            map.clear();       //每次重设A点时,清空Hash表
            map.put((double)Integer.MIN_VALUE,1);   //把相同点的K设为MIN_VALUE;
            int dup=0;
            for(int j=i+1;j<points.length;j++){
            //判断点是否重合
                if(points[j].x==points[i].x&&points[j].y==points[i].y){
                    dup++;
                    continue;
                }
            //判断斜率的K值.
                double key=points[j].x-points[i].x==0?
                        Integer.MAX_VALUE:
                            0.0+(double)(points[j].y-points[i].y)/(double)(points[j].x-points[i].x);

                if(map.containsKey(key)){
                    map.put(key,map.get(key)+1);
                }else{
                    map.put(key, 2);
                }
            }
            for(int temp:map.values()){
                if(temp+dup>max){
                    max=temp+dup;
                }
            }
        }
        return max;
    }



}

```
   
  