---
title: LintCode- 二叉树的所有路径
date: '2016-02-26T20:58:21.000Z'
tags: CSDN迁移
---

# LintCode- 二叉树的所有路径

版权声明：本文为博主原创文章，未经博主允许不得转载。 [https://blog.csdn.net/Sunny\_Ran/article/details/50752458](https://blog.csdn.net/Sunny_Ran/article/details/50752458)  
二叉树的所有路径

给一棵二叉树，找出从根节点到叶子节点的所有路径。  
样例  
给出下面这棵二叉树：

1  
/ \  
2 3  
\  
5  
所有根到叶子的路径为：

\[  
“1-&gt;2-&gt;5”,  
“1-&gt;3”  
\]

```text
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param root the root of the binary tree
     * @return all root-to-leaf paths
     */
    public List<String> binaryTreePaths(TreeNode root) {
            List<String> result = new ArrayList<String>();
            if(root==null) return result;
            String str = Integer.toString(root.val);
            findPaths(root,str,result);
            return result;
        }

        public void findPaths(TreeNode root, String str,List<String> result){
            String left= new String();
            String right = new String();
            if(root.left!=null)
            {
                left = str+"->"+Integer.toString(root.left.val);
                this.findPaths(root.left,left, result);
            }

            if(root.right!=null)
            {
                right = str+"->"+Integer.toString(root.right.val);
                this.findPaths(root.right,right,result);
            }
            if(root.right==null&&root.left==null) result.add(str);
        }
}
```

