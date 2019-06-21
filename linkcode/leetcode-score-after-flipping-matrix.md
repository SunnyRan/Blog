---
title: LeetCode-Score After Flipping Matrix
date: '2019-06-13T20:34:09.000Z'
tags: CSDN迁移
---

# LeetCode-Score After Flipping Matrix

We have a two dimensional matrix A where each value is 0 or 1.

A move consists of choosing any row or column, and toggling each value in that row or column: changing all 0s to 1s, and all 1s to 0s.

After making any number of moves, every row of this matrix is interpreted as a binary number, and the score of the matrix is the sum of these numbers.

Return the highest possible score.

```text
Example 1:

Input: [[0,0,1,1],[1,0,1,0],[1,1,0,0]]
Output: 39
Explanation:
Toggled to [[1,1,1,1],[1,0,0,1],[1,1,1,1]].
0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39


Note:

1 <= A.length <= 20
1 <= A[0].length <= 20
A[i][j] is 0 or 1.
```

**题目大意**  
题目中给了一个数组A，这个数组中只包含0,1。现在需要整行或者整列的进行转换操作，把0变成1，把1变成0。目标是进行一波t操作之后，把A中的每行数字转化成二进制数，是最终得到的二进制数的和最大。

解题方法  
题目很烧脑，使用什么样的操作规则才能使得得到的最终数组二进制和最大。  
1、根据二进制的特性，1000 一定大于 0111，所以首位数字一定要为1  
2、第一列都为1时，那么行的变化就不能进行了，因为一单进行行的变化，每行的首位就会变成0  
3、现在只需要对列进行操作就好，目标是保证每一列的1尽量多。

```text
    public int matrixScore(int[][] A) {
        int row = A.length;
        int col = A[0].length;
        int current = 1<<(A[0].length-1);
        int sum =current * row;
        int k;
        for(int i = 1; i< col;i++){
            k = 0;
            current = current>>1;
            for(int j = 0;j<row;j++){
                if((A[j][0]^A[j][i]) == 1){
                    k++;
                }
            }
            k = Math.max(k,row - k);
            sum += current * k;
        }
        return sum;
    }
```

技巧：  
1、避免对数组进行赋值操作，该为通过首列的值与第二列、第三列、…第n列进行异或操作。  
1）如果首列为0，则需要对后面进行转换操作，0^1 = 0 , 0^0=1  
2）如果首列为1，则无需进行转换操作， 1^1 = 1 , 1^0 = 0;  
2、统计每一列1的个数k，因为0和1可以相互转换，所以行数减k取其大值就是该列1的最大值 。k = Math.max\(k,row - k\);  
3\)根据二进制的转换数字求和。

