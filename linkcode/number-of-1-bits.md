# Number of 1 Bits



**Example 1:**

```text
Input: 00000000000000000000000000001011
Output: 3
Explanation: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.
```

**Example 2:**

```text
Input: 00000000000000000000000010000000
Output: 1
Explanation: The input binary string 00000000000000000000000010000000 has a total of one '1' bit.
```

**Example 3:**

```text
Input: 11111111111111111111111111111101
Output: 31
Explanation: The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits.
```

解题思路：  
直接使用位运算进行操作

代码：

```text
  public int hammingWeight(int n) {
    int re = 0;
    // 1、当移位的数是正数的时候，>> 和>>>效果都是一样的；
    // 2、当移位的数是负数的时候，>>将二进制高位用1补上，而>>>将二进制高位用0补上，这就导致了>>>将负数的移位操作结果变成了正数（因为高位用0补上了）
    for (; n != 0; n >>>= 1) {
      re += (n & 1);
    }
    return re;
  }
```

