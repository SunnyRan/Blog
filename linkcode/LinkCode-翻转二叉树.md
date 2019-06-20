---
title: LinkCode-翻转二叉树
date: 2015-08-30 01:06:43
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/48097357   
   #### 翻转二叉树

 样例 ```
  1         1
 / \       / \
2   3  => 3   2
   /       \
  4         4

```
   
   
 挑战 递归固然可行，能否写个非递归的？

   
   
   


 

 


```
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
     * @param root: a TreeNode, the root of the binary tree
     * @return: nothing
     */
       public void invertBinaryTree(TreeNode root) {
        if(root.left!=null||root.right!=null){
        	 TreeNode empy=root.left;
        	 root.left=root.right;
        	 root.right=empy;
        	 if(root.right!=null){
        		 invertBinaryTree(root.right);
             }
        	 if(root.left!=null){
        		 invertBinaryTree(root.left);
             }
        	
        }
     
    }
}
```
  
  
   
 