# invert-binary-tree-fan-zhuan-er-cha-shu

Invert a binary tree.

**Example:**

Input:

```text
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

Output:

```text
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

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

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

