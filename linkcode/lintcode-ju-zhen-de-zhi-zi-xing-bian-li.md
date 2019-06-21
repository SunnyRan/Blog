---
title: LintCode-矩阵的之字型遍历
date: '2015-11-08T21:36:27.000Z'
tags: CSDN迁移
---

# LintCode-矩阵的之字型遍历

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/49722099](https://blog.csdn.net/Sunny_Ran/article/details/49722099)

## 容易 矩阵的之字型遍历

给你一个包含 m x n 个元素的矩阵 \(m 行, n 列\), 求该矩阵的之字型遍历。

对于如下矩阵：

```text
[
  [1, 2,  3,  4],
  [5, 6,  7,  8],
  [9,10, 11, 12]
]
```

返回 `[1, 2, 5, 9, 6, 3, 4, 7, 10, 11, 8, 12]`

```text
public class Solution {
    /**
     * @param matrix: a matrix of integers
     * @return: an array of integers
     */ 
    public int[] printZMatrix(int[][] matrix) {
        if(matrix == null){
            return null;
        }
        int m = matrix[0].length;
        int n = matrix.length;
        int i = 0;
        int count = n*m;
        int [] re = new int [count];
        int a=0,b=0;
        re[i++] = matrix[a][b];
        if(n == 1){
            return matrix[0];
        }
        if(m == 1){
            int [] ree = new int [n];
            for(int k =0; k<n;k++){
                ree[k] = matrix[k][0];
            }
        }
        while(i <count){
            //斜向上遍历
            while(a > 0 && b+1 < m){
                re[i++] = matrix[--a][++b];
            }
            //斜向上结束，试着往右或者下移动
            if(b+1 < m){
                re[i++] = matrix[a][++b];
            }else{
                if(a+1 <n){
                    re[i++] = matrix[++a][b];
                }
            }
            //斜向下遍历
            while(a+1 < n && b > 0){
                re[i++] = matrix[++a][--b];
            }
            //斜向下遍历结束，试着往下或者往右移动
            if(a+1 < n){
                re[i++] = matrix[++a][b];
            }else{
                if(b + 1 <m){
                    re[i++] = matrix[a][++b];
                }
            }

        }
        return re;
    }
}
```

