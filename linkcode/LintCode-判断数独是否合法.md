---
title: LintCode-判断数独是否合法
date: 2015-10-24 12:29:30
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/49384089   
  容易 判断数独是否合法 查看运行结果 

 27% 通过   
 请判定一个数独是否有效。

 该数独可能只填充了部分数字，其中缺少的数字用 . 表示。

 您在真实的面试中是否遇到过这个题？ Yes   
 样例   
 下列就是一个合法数独的样例。

 Valid Sudoku

 注意   
 一个合法的数独（仅部分填充）并不一定是可解的。我们仅需使填充的空格有效即可。

 说明   
 什么是 数独？

 [http://sudoku.com.au/TheRules.aspx](http://sudoku.com.au/TheRules.aspx)   
 [http://baike.baidu.com/subview/961/10842669.htm](http://baike.baidu.com/subview/961/10842669.htm)

 
```
class Solution {
    /**
      * @param board: the board
        @return: wether the Sudoku is valid
      */
    public boolean isValidSudoku(char[][] board) {
        if(board==null)
            return false;
        for(int i=0;i<board.length;i++){
            HashMap<Character, Boolean> map = new HashMap<Character, Boolean>();
            for(int j=0;j<board.length;j++){
                if(board[i][j]!='.'){
                    if(map.get(board[i][j])!=null){
                        return false;
                    }else{
                        map.put(board[i][j], true);
                    }
                }
            }
            map.clear();
        }
        for(int i=0;i<board.length;i++){
            HashMap<Character, Boolean> map = new HashMap<Character, Boolean>();
            for(int j=0;j<board.length;j++){
                if(board[j][i]!='.'){
                    if(map.get(board[j][i])!=null){
                        return false;
                    }else{
                        map.put(board[j][i], true);
                    }
                }
            }
            map.clear();
        }

        for(int l=0;l<3;l++){   
            for(int k=0;k<3;k++){   
                HashMap<Character, Boolean> map = new HashMap<Character, Boolean>();
                for(int i=0;i<3;i++){
                    for(int j=0;j<3;j++){
                        if(board[j+l*3][i+3*k]!='.'){
                            if(map.get(board[j+l*3][i+3*k])!=null){
                                return false;
                            }else{
                                map.put(board[j+l*3][i+3*k], true);
                            }
                        }
                    }
                }
            }
        }




        return true;
    }
};

```
   
  