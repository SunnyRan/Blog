# Bitwise ORs of Subarrays



We have an array A of non-negative integers.

For every \(contiguous\) subarray B = \[A\[i\], A\[i+1\], …, A\[j\]\] \(with i &lt;= j\), we take the bitwise OR of all the elements in B, obtaining a result A\[i\] \| A\[i+1\] \| … \| A\[j\].

Return the number of possible results. \(Results that occur more than once are only counted once in the final answer.\)

Example 1:

Input: \[0\]  
Output: 1  
Explanation:  
There is only one possible result: 0.  
Example 2:

Input: \[1,1,2\]  
Output: 3  
Explanation:  
The possible subarrays are \[1\], \[1\], \[2\], \[1, 1\], \[1, 2\], \[1, 1, 2\].  
These yield the results 1, 1, 2, 1, 3, 3.  
There are 3 unique values, so the answer is 3.  
Example 3:

Input: \[1,2,4\]  
Output: 6  
Explanation:  
The possible results are 1, 2, 3, 4, 6, and 7.

Note:

1 &lt;= A.length &lt;= 50000  
0 &lt;= A\[i\] &lt;= 10^9

解决方案1：  
暴力求解，两层For循环，第一层为数组开始的位置，第二层为子数组的所有正向求或运算操作。  
举例：A\[n\]  
第一层For秩为j，则只计算A\[j\]~A\[n\]的所有正向或运算操作  
第二层For秩为i，用于计算A\[j\],A\[j\]\|A\[j+1\],A\[j\]\|A\[j+1\]\|A\[j+2\],A\[j\]\|…\|A\[n\]的结果  
时间复杂度：O\(n^2\)

```text
  // 80 / 83 test cases passed.  Status: Time Limit Exceeded
  //但是暴力求解超时
  public int subarrayBitwiseORs(int[] A) {
    int size = A.length;
    HashSet reSet = new HashSet();
    int[] ends = new int[size + 1];
    for (int i = 0; i < size; i++) {
      ends[i] = 0;
      for (int j = i; j < size; j++) {
        ends[j + 1] = ends[j] | A[j];
        reSet.add(ends[j + 1]);
      }
    }
    return reSet.size();
  }
```

解决方案2（方案借鉴于@花花酱博客）：  
ORs\(A\[i+1\]\)的结果为ORs\(A\[i\]\)中所有的元素与A\[i+1\]进行或操作的结果与ORs\(A\[i\]\)取交集，并且加上A\[i+1\]自身。  
这个方案可以利用上一个数组的结果，进行新的数据计算，并且因为去除了重复的结果，可以加速或运算的次数。  
时间复杂度的计算@花花酱给出了证明。

举例:

```text
Input: [1,1,2]
Output: 3
Explanation: 
The possible subarrays are [1], [1], [2], [1, 1], [1, 2], [1, 1, 2].
These yield the results 1, 1, 2, 1, 3, 3.
There are 3 unique values, so the answer is 3.
```

ORs\[0\] = \[1\] = {1}  
ORs\[1\] = { ORs\[0\] \| A\[1\], A\[1\] ,ORs\[0\] } = {1} \| 1 , 1 = {1}  
ORs\[2\] = { ORs\[1\] \| A\[2\] ,A\[2\] ,ORs\[1\] } = {1} \| 2 ,2 = {3,2,1}

> 证明： dp\[i\] = {A\[i\], A\[i\] \| A\[i – 1\], A\[i\] \| A\[i – 1\] \| A\[i – 2\], … , A\[i\] \| A\[i – 1\] \| … \| A\[0\]}这个序列单调递增，通过把A\[i\]中的0变成1。A\[i\]最多有32个0。所以这个集合的大小 &lt;= 32。
>
> e.g. 举例：Worst Case 最坏情况 A = \[8, 4, 2, 1, 0\] A\[i\] = 2^\(n-i\)。
>
> A\[5\] = 0，dp\[5\] = {0, 0 \| 1, 0 \| 1 \| 2, 0 \| 1 \| 2 \| 4, 0 \| 1 \| 2 \| 4 \| 8} = {0, 1, 3, 7, 15}.

```text
  // 83 / 83 test cases passed.
  // Status: Accepted
  // Runtime: 357 ms

  public int subarrayBitwiseORs2(int[] A) {
    HashSet<Integer> cur = new HashSet();
    HashSet<Integer> ans = new HashSet();
    for (int a : A) {
      HashSet nxt = new HashSet<Integer>();
      nxt.add(a);
      for (int b : cur) {
        nxt.add(a | b);
      }
      ans.addAll(nxt);
      cur = nxt;
    }
    return  ans.size();
  }
```

```text
//224ms的方案，使用一个Set，提高了效率
class Solution {
    public int subarrayBitwiseORs(int[] A) {
        // brute force solution
        int size;
        if (A == null || (size = A.length) == 0) return 0;
        if (size == 1) return 1;

        int sumAll = 0;
        for (int i = 0; i < size; i++) {
            sumAll |= A[i];
        }

        Set<Integer> set = new HashSet<>();

        for (int i = 0; i < size; i++) {
            int sum = 0;
            for (int j = i; j < size; j++) {
                sum |= A[j];
                set.add(sum);
                if (sum == sumAll) {
                    break;
                }
            }
        }
        return set.size();
    }
}
```

