# 寻找缺失的数

public class Solution { /\*\*

* @param nums: an array of integers
* @return: an integer

  \*/

    public int findMissing\(int\[\] nums\) {

  ```text
   int k=nums.length;
   int max=k*(k+1)/2;
   int sun=0;
   for (int i = 0; i < nums.length; i++) {
       sun+=nums[i];
   }
   return max-sun;
  ```

   }

  }

\`\`\` 样例N = 4 且序列为 \[0, 1, 3\] 时，缺失的数为2。

注意

可以改变序列中数的位置。

挑战

在数组上原地完成，使用O\(1\)的额外空间和O\(N\)的时间。

思路

1.样例所给的是一个递增的序列,但是题目却没有说序列一定递增.所以通过遍历查找相邻数差为2的方法不成立;

2.因为时间复杂度为O\(1\),不能通过新建一个数组填数的方法寻扎缺失的数;

3.想到了所有数列跟完整数列缺失的是即为两个数列和所差的数,所以有了求总和然后做差的思路.

