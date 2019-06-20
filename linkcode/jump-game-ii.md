# Jump Game II



### Description

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

### For example:

Given array A = \[2,3,1,1,4\]

The minimum number of jumps to reach the last index is 2. \(Jump 1 step from index 0 to 1, then 3 steps to the last index.\)

Note:  
You can assume that you can always reach the last index.

### Solution

思路：  
这里把问题转化为广度优先搜索算法\(BFS\)

> 广度优先搜索（也称宽度优先搜索，缩写BFS，以下采用广度来描述）是连通图的一种遍历策略。因为它的思想是从一个顶点V0开始，辐射状地优先遍历其周围较广的区域，故得名。
>
> 具体到这个问题。  
> 我们分别把每一个节点nums\[i\]的节点进行遍历，找出从nums\[i\]节点跳出后，能到的最远距离curFarthest。

代码如下：

```text
    public int jump(int[] nums) {
        int curFarthest = 0,curEnd = 0,jump=0;
        for(int i = 0 ; i<nums.length-1;i++)
        {
            curFarthest = Math.max(curFarthest,i+nums[i]);
            if(i==curEnd){
                jump++;
                curEnd = curFarthest;
            }
        }
        return jump;
    }
```

```text
//图解
//数组上面的标识'|'记作curEnd
//数组下面的标识'!'记作curFarthest
//jump 代表跳步数


//i = 0,jump = 0
 |
[2,3,1,1,4]
 !    


//i = 0,jump = 1
 |
[2,3,1,1,4]
     !

//i = 1,jump = 1
   |
[2,3,1,1,4]
         !

//i = 2,jump = 1
     |
[2,3,1,1,4]
         !

//i = 3,jump = 1
       |
[2,3,1,1,4]
         !

//i = 3,jump = 2
         |
[2,3,1,1,4]
         !
```

