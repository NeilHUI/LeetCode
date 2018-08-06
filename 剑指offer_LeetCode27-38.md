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

1. LeetCode**无

   > Description

   从上往下打印出二叉树的每个节点，同层节点从左至右打印。 

   > Code_Python

   ```python
   class Solution:
       def PrintFromTopToBottom(self,root):
           result = []
           if not root:return result
           temp_nodes = []
           temp_nodes.append(root)
           while temp_nodes:
               current_root = temp_nodes.pop(0)
           	result.append(current_root.val)
           	if current_root.left:
                   temp_nodes.append(root.left)
               if current_root.right:
                   temp_nodes.append(root.right)
            return result
               
   ```

   > Code_Java

   ```java
   /**
   public class TreeNode {
       int val = 0;
       TreeNode left = null;
       TreeNode right = null;
   
       public TreeNode(int val) {
           this.val = val;
   
       }
   
   }
   */
   public class Solution {
       public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
           ArrayList<Integer> resultList = new ArrayList<>();
           if(root==null){return resultList;}
           //用于暂时存放未遍历的节点
           Queue<TreeNode> queue = new LinkedList<TreeNode>();
           queue.offer(root);
           while(!queue.isEmpty()){
               TreeNode tempNode = queue.poll();
               resultList.add(tempNode.val);
               if(tempNode.left!=null){queue.offer(tempNode.left);}
               if(tempNode.right!=null){queue.offer(tempNode.right);}
           }
           return resultList;
       }
   }
   ```

> 面试题33：二叉搜索树的后序遍历序列

1. LeetCode**无

   > Description

   输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。 

   > Code_Python

   ```python
   # -*- coding:utf-8 -*-
   class Solution:
       def VerifySquenceOfBST(self, sequence):
           if not sequence or not len(sequence):return False
           root = sequence[-1]
           # 二叉搜索树中左子树节点的值小于根节点的值
           i = 0
           for i in range(len(sequence)):
               if sequence[i]>root:break
           # 二叉搜索树中右子树节点的值如果小于根节点的值，则返回false
           for s in sequence[i:-1]:
               if s<root:return False
           # 判断左子树是不是二叉树
           left_flag = True
           if i:
               left_flag = self.VerifySquenceOfBST(sequence[:i])
           # 判断右子树是不是二叉树
           right_flag = True
           if i<len(sequence)-1:
               right_flag = self.VerifySquenceOfBST(sequence[i:-1])
           return left_flag and right_flag
   ```

   > Code_Java

   ```java
   public class Solution {
        public boolean isBst(int[] arr, int start, int root) {
           if (start >= root)
               return true;
           int index = start;
           // 二叉搜索树中左子树节点的值小于根节点的值
           while (arr[index] < arr[root] && index<root) {
               index++;
           }
           // 判断右子树是否有数字小于root节点的值
           for (int i = index ; i < root-1; i++) {
               if (arr[i] < arr[root]) {
                   return false;
               }
           }
           // 遍历左右子树
           return isBst(arr, start, index - 1) && isBst(arr, index, root - 1);
       }
        public boolean VerifySquenceOfBST(int [] sequence) {
           if(sequence.length==0){
               return false;
           }
           return isBst(sequence,0,sequence.length-1);
       }
   
   }
   ```

> 面试题34：二叉树中和为某一值的路径

1. [Path Sum](https://leetcode.com/problems/path-sum) (112)

   > Description

   Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

   **Note:** A leaf is a node with no children.

   **Example:**

   Given the below binary tree and `sum = 22`,

   ```
         5
        / \
       4   8
      /   / \
     11  13  4
    /  \      \
   7    2      1
   ```

   return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.

   > Code_Python

   ```python
   # Definition for a binary tree node.
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Solution:
       def hasPathSum(self, root, sum):
           """
           :type root: TreeNode
           :type sum: int
           :rtype: bool
           """
           if not root:return False
           if not root.left and not root.right and root.val == sum :return True
           return self.hasPathSum(root.left,sum-root.val) or self.hasPathSum(root.right,sum-root.val)
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
       public boolean hasPathSum(TreeNode root, int sum) {
           if(root==null){return false;}
           if(root.left==null && root.right == null && root.val == sum){return true;}
           return hasPathSum(root.left,sum-root.val) || hasPathSum(root.right,sum-root.val);
       }
   }
   ```

2. [Path Sum II](https://leetcode.com/problems/path-sum-ii) (113)

   > Description

   Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

   **Note:** A leaf is a node with no children.

   **Example:**

   Given the below binary tree and `sum = 22`,

   ```
         5
        / \
       4   8
      /   / \
     11  13  4
    /  \    / \
   7    2  5   1
   ```

   Return:

   ```
   [
      [5,4,11,2],
      [5,8,4,5]
   ]
   ```

   > Code_Python

   ```python
   # Definition for a binary tree node.
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Solution:
       def pathSum(self, root, sum):
           """
           :type root: TreeNode
           :type sum: int
           :rtype: List[List[int]]
           """
           def helper(root_temp, sum_temp ,list_temp):
               if not root_temp.left and not root_temp.right and root_temp.val==sum_temp:
                   result_lists.append(list_temp)
               if root_temp.left:
                   helper(root_temp.left,sum_temp-root_temp.val,list_temp+[root_temp.left.val]) 
               if root_temp.right:
                   helper(root_temp.right,sum_temp-root_temp.val,list_temp+[root_temp.right.val]) 
           if not root:return []
           result_lists = []
           helper(root,sum,[root.val])
           return result_lists
       
   #非递归
   
   # Definition for a binary tree node.
   # class TreeNode(object):
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Solution(object):
       def pathSum(self, root, sum):
           """
           :type root: TreeNode
           :type sum: int
           :rtype: List[List[int]]
           """
           if not root:return []
           res = []
           queue = [(root, sum, [root.val])]
           while queue:
               curr, val, ls = queue.pop(0)
               if not curr.left and not curr.right and val == curr.val:
                   res.append(ls)
               if curr.left:
                   queue.append((curr.left, val-curr.val, ls+[curr.left.val]))
               if curr.right:
                   queue.append((curr.right, val-curr.val, ls+[curr.right.val]))
           return res
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
       public List<List<Integer>> pathSum(TreeNode root, int sum) {
            List<List<Integer>> List=new ArrayList<List<Integer>>();
   		 List<Integer> sub=new ArrayList<Integer>();
   		 helperDFS(root,sum,List,sub);
   		 return List;
   	 }
   	 private void helperDFS(TreeNode root,int sum,List<List<Integer>> List, List<Integer> sub ){
   		 if(root==null) return;
   		 
   		 sub.add(root.val);
   		 //the case of reach the bottom leaf of tree
   		 if(sum==root.val && root.left==null && root.right==null){
   			 //Insert a clone of sub into List
   			 List.add(new ArrayList<Integer>(sub));
   		 }
   		 //Recursively through left and right sub tree
   		 helperDFS(root.left,sum-root.val,List,sub);
   		 helperDFS(root.right,sum-root.val,List,sub);
   		 //use Backtracking to deal with if next move is not fit, go back
   		 sub.remove(sub.size()-1);
   	}
       
   }
   ```

3. [Path Sum III](https://leetcode.com/problems/path-sum-iii) (437)  

   > Description

   You are given a binary tree in which each node contains an integer value.

   Find the number of paths that sum to a given value.

   The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

   The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

   **Example:**

   ```
   root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
   
         10
        /  \
       5   -3
      / \    \
     3   2   11
    / \   \
   3  -2   1
   
   Return 3. The paths that sum to 8 are:
   
   1.  5 -> 3
   2.  5 -> 2 -> 1
   3. -3 -> 11
   ```

   > Code_Python

   ```python
   # Definition for a binary tree node.
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Solution:
       def pathSum(self, root, sum):
           """
           :type root: TreeNode
           :type sum: int
           :rtype: int
           """
           def helper(root,sum):
               res = 0
               if not root:return res
               if sum==root.val:res+=1
               res+=helper(root.left,sum-root.val)
               res+=helper(root.right,sum-root.val)
               return res
           if not root:return 0
           return helper(root,sum)+self.pathSum(root.left,sum)+self.pathSum(root.right,sum)
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
       public int pathSum(TreeNode root, int sum) {
           if(root == null){
               return 0;
           }
           return dfs(root,sum)+pathSum(root.left,sum)+pathSum(root.right,sum);
           
       }
       private int dfs(TreeNode root,int sum){
           int res = 0;
           if(root == null){
               return res;
           }
           if(root.val ==sum){
               res++;
           }
           res+=dfs(root.left,sum-root.val);
           res+=dfs(root.right,sum-root.val);
           return res;
           
       }
   }
   ```

###### 4.4 分解让复杂问题简单化

> 面试题35：复杂链表的复制

1. [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer) (138)

   > Description

   A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

   Return a deep copy of the list.

   > Code_Python

   ```python
   # Definition for singly-linked list with a random pointer.
   # class RandomListNode(object):
   #     def __init__(self, x):
   #         self.label = x
   #         self.next = None
   #         self.random = None
   
   class Solution(object):
       def copyRandomList(self, head):
           """
           :type head: RandomListNode
           :rtype: RandomListNode
           """
           clone_dict = {}
           clone_head = head
           while clone_head:
               clone_dict[clone_head]=RandomListNode(clone_head.label)
               clone_head = clone_head.next
           clone_head = head
           while clone_head:
               clone_dict[clone_head].next = clone_dict[clone_head.next] if clone_head.next else None
               clone_dict[clone_head].random = clone_dict[clone_head.random] if clone_head.random else None
               clone_head = clone_head.next
           return clone_dict[head] if head else None
   ```

   > Code_Java

   ```java
   /**
    * Definition for singly-linked list with a random pointer.
    * class RandomListNode {
    *     int label;
    *     RandomListNode next, random;
    *     RandomListNode(int x) { this.label = x; }
    * };
    */
   public class Solution {
       public RandomListNode copyRandomList(RandomListNode head) {
           if (head == null){
               return null;
           }
           Map<RandomListNode,RandomListNode> cloneMap = new HashMap<>();
           RandomListNode cloneNode = head;
           while(cloneNode!=null){
               cloneMap.put(cloneNode,new RandomListNode(cloneNode.label));
               cloneNode = cloneNode.next;
               
           }
           cloneNode = head;
           while(cloneNode!=null){
               if(cloneNode.next!=null){
                   cloneMap.get(cloneNode).next = cloneMap.get(cloneNode.next);
               }else{
                   cloneMap.get(cloneNode).next = null;
               }
               if(cloneNode.random!=null){
                   cloneMap.get(cloneNode).random = cloneMap.get(cloneNode.random);
               }else{
                   cloneMap.get(cloneNode).random = null;
               }
               cloneNode = cloneNode.next;
               
           }
           return cloneMap.get(head);
       }
   }
   ```

> 面试题36：二叉搜索树与双向链表

1. LeetCode**无

   > Description

   输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。 

   > Code_Python

   ```python
   # -*- coding:utf-8 -*-
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   class Solution:
       def Convert(self, pRootOfTree):
           # write code here
           def ConvertNode(pRootOfTree):
               if not pNode:return
               ConvertNode(pRootOfTree.left)
               if not head:
                   head = pRootOfTree
                   realHead = pRootOfTree
               else:
                   head.right = pRootOfTree
                   pRootOfTree.left = head
                   head = pRootOfTree
               ConvertSub(pRootOfTree.right)
           head = None
           realHead = None
           ConvertNode(pRootOfTree)
           return pHeadOfList
   ```

   > Code_Java

   ```java
   /**
   public class TreeNode {
       int val = 0;
       TreeNode left = null;
       TreeNode right = null;
   
       public TreeNode(int val) {
           this.val = val;
   
       }
   
   }
   */
   public class Solution {
       TreeNode head = null;
       TreeNode realHead = null;
       public TreeNode Convert(TreeNode pRootOfTree) {
           ConvertSub(pRootOfTree);
           return realHead;
       }
        
       private void ConvertSub(TreeNode pRootOfTree) {
           if(pRootOfTree==null) return;
           ConvertSub(pRootOfTree.left);
           if (head == null) {
               head = pRootOfTree;
               realHead = pRootOfTree;
           } else {
               head.right = pRootOfTree;
               pRootOfTree.left = head;
               head = pRootOfTree;
           }
           ConvertSub(pRootOfTree.right);
       }
   }
   ```

> 面试题37：序列化二叉树

1. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree) (297)

   > Description

   Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

   Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

   **Example:** 

   ```
   You may serialize the following tree:
   
       1
      / \
     2   3
        / \
       4   5
   
   as "[1,2,3,null,null,4,5]"
   ```

   **Clarification:** The above format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

   **Note:** Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

   > Code_Python

   ```python
   # Definition for a binary tree node.
   # class TreeNode(object):
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Codec:
   
       def serialize(self, root):
           """Encodes a tree to a single string.
           
           :type root: TreeNode
           :rtype: str
           """
           if not root:
               return ''
           def dfs(node):
               if node:
                   res.append(str(node.val))
                   dfs(node.left)
                   dfs(node.right)
               else:
                   res.append("#")
           res = []
           dfs(root)
           return ' '.join(res)
   
   
           
   
       def deserialize(self, data):
           """Decodes your encoded data to tree.
           
           :type data: str
           :rtype: TreeNode
           """
           if len(data) == 0:
               return None
           def dfs():
               val = next(res)
               if val == '#':
                   return None
               node = TreeNode(int(val))
               node.left = dfs()
               node.right = dfs()
               return node
           res = iter(data.split())
           return dfs()
   
           
   
   # Your Codec object will be instantiated and called as such:
   # codec = Codec()
   # codec.deserialize(codec.serialize(root))
   ```

   > Code_Java

   ```java
   
   ```

> 面试题38：字符串的排列

1. LeetCode**无

   > Description

   输入一个字符串，打印出该字符串中字符的所有排列。 

   例如输入字符串abc，则打印由字符a,b,c所能排列出来的所有字符串：abc，abc,bac,bca,cab,cba 

   > Code_Python

   ```python
   class Solution:
       def permutation(self,arr):
           if not arr:return
           res = []
           self.helper(arr, res, '')
           return sorted(list(set(res)))
       def helper(self, arr, res, path):
           if not arr:
               res.append(path)
           else:
               for i in range(len(arr)):
                   self.helper(arr[:i] + arr[i+1:], res, path + arr[i])
   ```

   