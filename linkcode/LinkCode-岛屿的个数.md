---
title: LinkCode-岛屿的个数
date: 2015-08-31 16:26:22
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/48135845   
  岛屿的个数 

 给一个01矩阵，求不同的岛屿的个数。

 0代表海，1代表岛，如果两个1相邻，那么这两个1属于同一个岛。我们只考虑上下左右为相邻。

 样例   
 在矩阵：

 [   
 [1, 1, 0, 0, 0],   
 [0, 1, 0, 0, 1],   
 [0, 0, 0, 1, 1],   
 [0, 0, 0, 0, 0],   
 [0, 0, 0, 0, 1]   
 ]   
 中有 3 个岛.

 问题分析:   
 1.这里采用暴力遍历的方法,遍历矩阵里面每一个元素.   
 2.发现岛屿后,运行find方法去找它的上下左右元素,判断是否为岛屿,是的话把true设为false,避免重复查找(递归)   
 3.知道查找完所有周围点为false后,结束find.   
 4.继续查找下一个岛屿.

 
```
public class Solution {
    /**
     * @param grid a boolean 2D matrix
     * @return an integer
     */
    public int numIslands(boolean[][] grid) {
        int k=0;
        for (int i = 0; i < grid.length; i++) {
            for(int j=0;j<grid[0].length;j++){
                if(grid[i][j]){
                    k++;
                    find(i,j,grid);
                }
            }
        }
        return k;

    }


    public  void find(int i,int j,boolean[][] grid){
        grid[i][j]=false;
        if(i+1<grid.length&&grid[i+1][j]){
            find(i+1,j,grid);
        }
        if(j+1<grid[0].length&&grid[i][j+1]){
            find(i,j+1,grid);
        }
        if(i>0&&grid[i-1][j]){
            find(i-1,j,grid);
        }
        if(j>0&&grid[i][j-1]){
            find(i,j-1,grid);
        }

    }
}

```
   
  