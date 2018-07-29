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

1. [Min Stack](https://leetcode.com/problems/min-stack) (155)

   > Description

   Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

   - push(x) -- Push element x onto stack.
   - pop() -- Removes the element on top of the stack.
   - top() -- Get the top element.
   - getMin() -- Retrieve the minimum element in the stack.

   **Example:**

   ```
   MinStack minStack = new MinStack();
   minStack.push(-2);
   minStack.push(0);
   minStack.push(-3);
   minStack.getMin();   --> Returns -3.
   minStack.pop();
   minStack.top();      --> Returns 0.
   minStack.getMin();   --> Returns -2.
   ```

   > Code_Python

   ```python
   class MinStack:
   
       def __init__(self):
           """
           initialize your data structure here.
           """
           self._min_stack = []
           self._patch_min_stack = []
   
       def push(self, x):
           """
           :type x: int
           :rtype: void
           """
           self._min_stack.append(x)
           self._patch_min_stack.append(x) if ( not len(self._patch_min_stack) or self._patch_min_stack[-1] > x) else self._patch_min_stack.append(self._patch_min_stack[-1])
   
       def pop(self):
           """
           :rtype: void
           """
           self._patch_min_stack.pop()
           return self._min_stack.pop()
   
       def top(self):
           """
           :rtype: int
           """
           return self._min_stack[-1]
   
       def getMin(self):
           """
           :rtype: int
           """
           return self._patch_min_stack[-1] if len(self._patch_min_stack) else None
           
   
   
   # Your MinStack object will be instantiated and called as such:
   # obj = MinStack()
   # obj.push(x)
   # obj.pop()
   # param_3 = obj.top()
   # param_4 = obj.getMin()
   ```

   > Code_Java

   ```java
   class MinStack {
   
       Stack<Integer> stack;
       //存放最小值列表
       Stack<Integer> patchStack;
       /** initialize your data structure here. */
       public MinStack() {
           stack = new Stack<Integer>();
           patchStack = new Stack<Integer>();
       }
       
       public void push(int x) {
           stack.push(x);
           if(patchStack.empty() || patchStack.peek()>x){
               patchStack.push(x);
           }else{
               patchStack.push(patchStack.peek());
           }
       }
       
       public void pop() {
           patchStack.pop();
           stack.pop();
       }
       
       public int top() {
           return stack.peek();
       }
       
       public int getMin() {
           return patchStack.peek();
           
       }
   }
   
   /**
    * Your MinStack object will be instantiated and called as such:
    * MinStack obj = new MinStack();
    * obj.push(x);
    * obj.pop();
    * int param_3 = obj.top();
    * int param_4 = obj.getMin();
    */
   ```

> 面试题31：栈的压入、弹出序列

1. LeetCode**无

   > Description

   输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4，5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。

   > Code_Python

   ```python
   class Solution:
       def IsPopOrder(self, pushV, popV):
           # write code here
           stack = []
           if not pushV and popV:return True
           patch_stack = [x for x in reversed(pushV)]
           print(patch_stack)
           for pop_a in popV:
               if stack and pop_a == stack[-1]:
                   stack.pop()
               else:
                   if pop_a not in patch_stack:return False
                   else:
                       while True:
                           temp = patch_stack.pop()
                           if temp == pop_a: break
                           stack.append(temp)
           return not stack
   ```

   > Code_Java

   ```java
   public class Solution {
       public boolean IsPopOrder(int [] pushA,int [] popA) {
           if(pushA.length == 0 || popA.length == 0)
               return false;
           Stack<Integer> s = new Stack<Integer>();
           //用于标识弹出序列的位置
           int popIndex = 0;
           for(int i = 0; i< pushA.length;i++){
               s.push(pushA[i]);
               //如果栈不为空，且栈顶元素等于弹出序列
               while(!s.empty() &&s.peek() == popA[popIndex]){
                   //出栈
                   s.pop();
                   //弹出序列向后一位
                   popIndex++;
               }
           }
           //如果最后辅助栈为空则匹配成功
           return s.empty();
       }
   }
   ```

> 面试题32：从上到下打印二叉树

​	

​		





