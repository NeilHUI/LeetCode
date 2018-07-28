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

> 面试题28：对称的二叉树

1. [Symmetric Tree](https://leetcode.com/problems/symmetric-tree) (101)

   Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

   For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:

   ```
       1
      / \
     2   2
    / \ / \
   3  4 4  3
   ```

   But the following `[1,2,2,null,3,null,3]` is not:

   ```
       1
      / \
     2   2
      \   \
      3    3
   ```

   **Note:**
   Bonus points if you could solve it both recursively and iteratively.

   > Code_Python

   ```python
   # Definition for a binary tree node.
   # class TreeNode(object):
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Solution(object):
       def isSymmetric(self, root):
           def is_same(root_left,root_right):
               if not root_left and not root_right:return True
               if not root_left or not root_right or root_left.val!=root_right.val:return False 
               return is_same(root_left.left,root_right.right) and is_same(root_left.right,root_right.left)
           """
           :type root: TreeNode
           :rtype: bool
           """
           if not root:return True
           return is_same(root.left,root.right)
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
       public boolean isSymmetric(TreeNode root) {
           //递归两条件，递归体，以及递归停止条件
           if (root==null){return true;}
           //将根节点分为左右子树进行判断
           return isSame(root.left,root.right);
       }
       public boolean isSame(TreeNode rootLeft,TreeNode rootRight){
           //递归停止条件，如果两个都为空，则为True
           if(rootLeft == null && rootRight == null){
               return true;
           }
           //递归False停止条件
           if(rootLeft == null || rootRight==null || rootLeft.val != rootRight.val){
               return false;
           }
           //分别递归左子树的左节点和右子树的右节点，以及左子树的右节点和右子树的左节点
           return isSame(rootLeft.left,rootRight.right) && isSame(rootLeft.right,rootRight.left);
       }
   }
   ```

> 面试题29：顺时针打印矩阵

1. [Spiral Matrix](https://leetcode.com/problems/spiral-matrix) (54)

   Given a matrix of *m* x *n* elements (*m* rows, *n* columns), return all elements of the matrix in spiral order.

   **Example 1:**

   ```
   Input:
   [
    [ 1, 2, 3 ],
    [ 4, 5, 6 ],
    [ 7, 8, 9 ]
   ]
   Output: [1,2,3,6,9,8,7,4,5]
   ```

   **Example 2:**

   ```
   Input:
   [
     [1, 2, 3, 4],
     [5, 6, 7, 8],
     [9,10,11,12]
   ]
   Output: [1,2,3,4,8,12,11,10,9,5,6,7] 
   ```

   > Code_Python

   ```python
   class Solution:
       def spiralOrder(self, matrix):
           """
           :type matrix: List[List[int]]
           :rtype: List[int]
           """
           result = []
           if not matrix or not matrix[0]:return result
           left, right, top, bottom = 0,len(matrix[0])-1,0,len(matrix)-1
           while left<=right and top<=bottom:
               for i in range(left,right+1):
                   result.append(matrix[top][i])
               for j in range(top+1,bottom):
                   result.append(matrix[j][right])
               for n in reversed(range(left,right+1)):
                   if top < bottom:
                       result.append(matrix[bottom][n])
               for m in reversed(range(top+1,bottom)):
                   if left<right:
                       result.append(matrix[m][left])
               left,right,top,bottom = left+1,right-1,top+1,bottom-1
           return result
   ```

   > Code_Java

   ```java
   public class Solution {
       public List<Integer> spiralOrder(int[][] matrix) {
           List<Integer> res = new ArrayList<>();
           if(matrix == null || matrix.length == 0)
               return res;
           int rowNum = matrix.length, colNum = matrix[0].length;
           int left = 0, right = colNum - 1, top = 0, bot = rowNum - 1;
           while(res.size() < rowNum * colNum) {
               for(int col = left; col <= right; col++)
                   res.add(matrix[top][col]);
               top++;
               if(res.size() < rowNum * colNum) {
                   for(int row = top; row <= bot; row++)
                       res.add(matrix[row][right]);
                   right--;   
               }
               if(res.size() < rowNum * colNum) {
                   for(int col = right; col >= left; col--)
                       res.add(matrix[bot][col]);
                   bot--;
               }
               if(res.size() < rowNum * colNum) {
                   for(int row = bot; row >= top; row--)
                       res.add(matrix[row][left]);
                   left++;
               }
           }
            
           return res;
       }
   }　　
   ```

###### 4.3举例让抽象问题具体化

> 面试题30：包含min函数的栈