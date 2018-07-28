### 剑指offer——LeetCode~27-38

##### 4 解决面试题的思路

###### 4.2 画图让抽象问题形象化

> 面试题27：二叉树的镜像

1. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree) (226)

   Invert a binary tree.

   **Example:**

   Input:

   ```
        4
      /   \
     2     7
    / \   / \
   1   3 6   9
   ```

   Output:

   ```
        4
      /   \
     7     2
    / \   / \
   9   6 3   1
   ```

   **Trivia:**
   This problem was inspired by [this original tweet](https://twitter.com/mxcl/status/608682016205344768) by [Max Howell](https://twitter.com/mxcl):

   > Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.

   > Code_Python

   ```python
   # Definition for a binary tree node.
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Solution:
       def invertTree(self, root):
           """
           :type root: TreeNode
           :rtype: TreeNode
           """
           if not root:return None
           left = root.left
           root.left = self.invertTree(root.right)
           root.right = self.invertTree(left)
           return root
   # 非递归的做法
   class Solution:
       def invertTree(self, root):
           """
           :type root: TreeNode
           :rtype: TreeNode
           """
           if not root:return root
           tree_list = []
           tree_list.append(root)
           while len(tree_list)>0:
               root_sub = tree_list.pop()
               root_left = root_sub.left
               root_sub.left = root_sub.right
               root_sub.right = root_left
               if root_sub.left:tree_list.append(root_sub.left)
               if root_sub.right:tree_list.append(root_sub.right)
           return root
   ```

   > Code_Java

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode(int x) { val = x; }
    * }
    */
   class Solution {
       public TreeNode invertTree(TreeNode root) {
           if(root==null){return null;}
           //创建临时的之前的左节点
           TreeNode preLeft = root.left;
           root.left = invertTree(root.right);
           root.right = invertTree(preLeft);
           return root
               
       }
   }
   //非递归做法请参考python部分
   ```

   