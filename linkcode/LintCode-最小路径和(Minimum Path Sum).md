---
title: LintCode-最小路径和(Minimum Path Sum)
date: 2015-11-11 23:44:33
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/49789523   
  最小路径和

 给定一个只含非负整数的m*n网格，找到一条从左上角到右下角的可以使数字和最小的路径。

 样例  
 注意  
 你在同一时间只能向下或者向右移动一步

 Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

 Note: You can only move either down or right at any point in time.

 Example:

 
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.

```
 
```
public class Solution {
    /**
     * @param grid: a list of lists of integers.
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    public int minPathSum(int[][] grid) {
		int n = grid.length;
		int m = grid[0].length;
		int a=0,b=0;
		for( a =0; a<n; a++){
			for( b=0; b<m; b++){
				if(a==0){
					grid[0][b]+=b>0?grid[0][b-1]:0;
				}
				if(b==0&&a!=0){
					grid[0][b]+= grid[a][b];
				}else{
					if(a!=0)
						grid[0][b]=grid[a][b]+(grid[0][b]>grid[0][b-1]?grid[0][b-1]:grid[0][b]);
				}
			}
		}
		return grid[0][m-1]; 
	}
}



//四年后又一次刷到这道题，这次一次AC
//给出思路
// 1、如果是1x1的格子，最小路径是他本身
// 2、现在扩充为是2x2的格子，最小路径为min([0][0]+[0][1],[1][0])+[1][1]
// 3、现在扩征为2x3的格子，最小路径为min([2,0],[1,1])+[1][2]
// 4、、、、、、
// 5、、、、、、、

//由此推断，到达[n][m]的最小值，取决于到达[n-1][m]、[n][m-1]的最小值


    public int minPathSum(int[][] grid) {
        int iMax = grid.length;
        int jMax = grid[0].length;
        for(int i =0 ;i<iMax;i++){
            for (int j =0;j<jMax;j++){
                //第一行处理
                if(i == 0 ){
                    if(j>0) {
                        grid[i][j] = grid[i][j] + grid[i][j - 1];
                    }
                }
                //非第一行处理
                if(i>0){
                    if(j == 0){
                        grid[i][j] = grid[i][j] + grid[i-1][j];
                    }else{
                        grid[i][j] = grid[i][j] + Math.min(grid[i-1][j],grid[i][j-1]);
                    }
                }
            }
        }
        return grid[iMax-1][jMax-1];
    }

```
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190612203153573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1N1bm55X1Jhbg==,size_16,color_FFFFFF,t_70)

   
  