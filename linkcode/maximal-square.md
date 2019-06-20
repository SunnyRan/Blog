# Maximal Square



**Maximal Square**

**Given a 2D binary matrix filled with 0's and 1's, find the largest square containing all 1's and return its area.**

样例 For example, given the following matrix:

```text
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

Return `4` .

```text
S[i][j] = min(S[i][j-1], S[i-1][j], S[i-1][j-1]) + 1
```

```text
public class Solution {
        /**
         * @param matrix: a matrix of 0 and 1
         * @return: an integer
         */
        public int maxSquare(int[][] matrix) {
            int max=0;
            if(matrix==null){
                return 0;
            }
            else{
                if(matrix.length==1||matrix[0].length==1){
                    for (int[] i : matrix){
                         for (int j : i){
                              if(j==1){
                                  max=1;
                         }
                    }
                }
                }else{
                    for(int i=1;i<matrix.length;i++){
                        for(int j=1;j<matrix[0].length;j++){
                            if(matrix[i][j]==1){
                                matrix[i][j]=FindMax(matrix[i-1][j],matrix[i][j-1],matrix[i-1][j-1]);
                                if(max<matrix[i][j]){
                                    max=matrix[i][j];
                                }
                            }else{
                                matrix[i][j]=0;
                            }
                        }
                    }
                }
            }
            return max*max;
        }

        public int FindMax(int a,int b,int c){
            int max=a;
            if(b<max){
                max=b;
            }else{
                if(c<max){
                    max=c;
                }
            }
            return max+1;

        }
    }
```

