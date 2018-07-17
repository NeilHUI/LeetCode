### 剑指offer——LeetCode

##### 2.3 数据结构

###### 2.3.1 数组

> 面试题3 数组中重复的数字

 1. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate)  (217)

    > Description

    Given an array of integers, find if the array contains any duplicates.

    Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

    **Example 1:**

    ```
    Input: [1,2,3,1]
    Output: true
    ```

    **Example 2:**

    ```
    Input: [1,2,3,4]
    Output: false
    ```

    **Example 3:**

    ```
    Input: [1,1,1,3,3,4,3,2,4,2]
    Output: true
    ```

    > Code_Python

    ```python
    class Solution:
        def containsDuplicate(self, nums):
            """
            :type nums: List[int]
            :rtype: bool
            """
            return not(len(nums) == len(set(nums)))
    ```

    > Code_Java

    ```java
    class Solution {
        public boolean containsDuplicate(int[] nums) {
            if (nums == null || nums.length == 0) {
                return false;
            }
            Arrays.sort(nums);
            for (int i = 0; i < nums.length-1; i++) {
                if (nums[i]==nums[i+1]){
                    return true;
                }
            }
            return false;
        }
    }
    ```

 2. [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii)  (219)

    > Description

    Given an array of integers and an integer *k*, find out whether there are two distinct indices *i* and *j* in the array such that **nums[i] = nums[j]** and the **absolute** difference between *i* and *j* is at most *k*.

    **Example 1:**

    ```
    Input: nums = [1,2,3,1], k = 3
    Output: true
    ```

    **Example 2:**

    ```
    Input: nums = [1,0,1,1], k = 1
    Output: true
    ```

    **Example 3:**

    ```
    Input: nums = [1,2,3,1,2,3], k = 2
    Output: false
    ```

    > Code_Python

    ```python
    class Solution:
        def containsNearbyDuplicate(self, nums, k):
            """
            :type nums: List[int]
            :type k: int
            :rtype: bool
            """
            dict = {}
            for i,num in enumerate(nums):
                if num in dict:
                    if (i-dict[num])<=k:
                        return True
                dict[num]=i
            return False
    ```

    > Code_Java

    ```java
    class Solution {
        public boolean containsNearbyDuplicate(int[] nums, int k) {
            Map<Integer, Integer> tempMap = new HashMap<>();
            for (int i = 0; i < nums.length; i++) {
                if (tempMap.containsKey(nums[i])) {
                    if (i-tempMap.get(nums[i])<=k){
                        return true;
                    }
                }
                tempMap.put(nums[i], i);
            }
            return false;
        }
    }
    ```

    

 3. [Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii)  (220)

    > Description

    Given an array of integers, find out whether there are two distinct indices *i* and *j* in the array such that the **absolute** difference between **nums[i]** and **nums[j]** is at most *t* and the **absolute** difference between *i* and *j* is at most *k*.

    **Example 1:**

    ```
    Input: nums = [1,2,3,1], k = 3, t = 0
    Output: true
    ```

    **Example 2:**

    ```
    Input: nums = [1,0,1,1], k = 1, t = 2
    Output: true
    ```

    **Example 3:**

    ```
    Input: nums = [1,5,9,1,5,9], k = 2, t = 3
    Output: false
    ```

    > Code_Python

    ```python
    class Solution:
        def containsNearbyAlmostDuplicate(self, nums, k, t):
            """
            :type nums: List[int]
            :type k: int
            :type t: int
            :rtype: bool
            """
            if t == 0 and len(nums) == len(set(nums)):
                return False
            for i in range(len(nums)):
                for j in range(i+1, i+k+1):
                    if j >= len(nums): break
                    if abs(nums[i] - nums[j]) <= t:
                        return True
            return False
    ```

    > Code_Java

    ```java
    class Solution {
        public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
            if(nums == null || nums.length < 2 || k < 1 || t<0 ) {
                return false;
            }
            TreeSet<Long> set = new TreeSet<>();
            for(int i = 0; i < nums.length; i++){
                long l = (long) nums[i];
                //floor(E e) 方法返回在这个集合中小于或者等于给定元素的最大元素，如果不存在这样的元素,返回null.
                Long floor = set.floor(l);
                //ceiling(E e) 方法返回在这个集合中大于或者等于给定元素的最小元素，如果不存在这样的元素,返回null.
                Long ceil = set.ceiling(l);
                if((floor != null && l - floor <= t) ||(ceil != null && ceil - l <= t)){
                    return true;
                }
                set.add(l);
                if(i >= k)
                    set.remove((long)nums[i -k]);
            }
            return false;
        }
    } 
    ```

> 面试题4 二维数组中的查找

1. [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix)  (74)

   > Description

   Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

   - Integers in each row are sorted from left to right.
   - The first integer of each row is greater than the last integer of the previous row.

   **Example 1:**

   ```
   Input:
   matrix = [
     [1,   3,  5,  7],
     [10, 11, 16, 20],
     [23, 30, 34, 50]
   ]
   target = 3
   Output: true
   ```

   **Example 2:**

   ```
   Input:
   matrix = [
     [1,   3,  5,  7],
     [10, 11, 16, 20],
     [23, 30, 34, 50]
   ]
   target = 13
   Output: false
   ```

   > Code_python

   ```python
   class Solution:
       def searchMatrix(self, matrix, target):
           """
           :type matrix: List[List[int]]
           :type target: int
           :rtype: bool
           """
           if len(matrix)==0:
               return False
           i = 0
           j = len(matrix[0])-1
           while i<len(matrix) and j >= 0:
               if matrix[i][j] == target:return True
               elif matrix[i][j] > target:j-=1
               else: i+=1
           return False
   ```

   > Code_Java

   ```java
   class Solution {
       public boolean searchMatrix(int[][] matrix, int target) {
           if (matrix.length==0){
               return false;
           }
           int i = 0;
           int j= matrix[0].length-1;
           while(i<matrix.length && j>=0){
               if (matrix[i][j]==target){
                   return true;
               }
               else if (matrix[i][j]<target){
                   i+=1;
               }
               else {j-=1;}
           }
           return false;
       }
   }
   ```

2. [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii)  (240)

   > Description

   Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

   - Integers in each row are sorted in ascending from left to right.
   - Integers in each column are sorted in ascending from top to bottom.

   **Example:**

   Consider the following matrix:

   ```
   [
     [1,   4,  7, 11, 15],
     [2,   5,  8, 12, 19],
     [3,   6,  9, 16, 22],
     [10, 13, 14, 17, 24],
     [18, 21, 23, 26, 30]
   ]
   ```

   Given target = `5`, return `true`.

   Given target = `20`, return `false`.

   > Code_Python

   ```java
   class Solution:
       def searchMatrix(self,matrix, target):
           """
           :type matrix: List[List[int]]
           :type target: int
           :rtype: bool
           """
   	    if not matrix:
               return False
           if not matrix[0]:
               return False
           rows, cols = len(matrix), len(matrix[0])
           r, c = 0, cols - 1
           while r < rows and c > -1:
               if matrix[r][c] == target:
                   return True
               elif matrix[r][c] > target:
                   c -= 1
               else:
                   r += 1
           return False
   ```

   > Code_Java

   ```java
   class Solution {
       public boolean searchMatrix(int[][] matrix, int target) {
           if (matrix.length==0 || matrix[0].length==0){
               return false;
           }
           int rows = matrix.length;
           int cols = matrix[0].length;
           int r = 0;
           int c = cols-1;
           while (r < rows && c > -1) {
               if (matrix[r][c]==target){
                   return true;
               } else if (matrix[r][c] > target) {
                   c -= 1;
               }else {
                   r+=1;
               }
           }
           return false;
       }
   }
   ```

###### 2.3.2 字符串

> 面试题5 替换空格 

1. **leetcode 无

   > Description

   请实现一个函数，把字符串中的每个空格替换成“%20”.例如，输入“we are happy.”，则输出“we%20are%20happy”

   > Code_Python

   ```python
   class Solution:
       def replaceSpace(self, str):
           # write code here
           return str.replace(" ","%20")
   ```

   > Java_Code

   ```java
   //时间复杂度为o(n)
   class Solution{
       public static String replaceSpace2(StringBuffer str){
                   if (string==null){
               return null;
           }
           int oldLength=string.length;
           int newLength = 0;
           int spaceNum = 0;
           for (char c : string) {
               if (c==' ') {
                   spaceNum++;
               }
           }
           newLength = spaceNum*2+oldLength;
           char[] tempArray = new char[newLength];
           int indexOld = oldLength-1;
           int indexNew = newLength-1;
           while (indexOld >= 0 && indexOld != indexNew) {
               if(string[indexOld]==' '){
                   tempArray[indexNew--]='0';
                   tempArray[indexNew--]='2';
                   tempArray[indexNew--]='%';
               }else {
                   tempArray[indexNew--] =string[indexOld];
               }
               indexOld--;
           }
           for (int i = indexOld; i >=0; i--) {
               tempArray[indexNew--] =string[i];
   
           }
           StringBuilder stringBuilder = new StringBuilder();
           for (char c : tempArray) {
               stringBuilder.append(c);
           }
           return stringBuilder.toString();
       }
   } 
   ```

###### 2.3.3 链表

> 面试题6 从尾到头打印链表

1. **LeetCode 无

   > Description

   题目：输入一个链表的头结点，从尾到头反过来打印出每个结点的值。 

   > Code_Python

   ```python
   class Solution:
       # 返回从尾部到头部的列表值序列，例如[1,2,3]
       def printListFromTailToHead(self, listNode):
           newlist =[]
           while listNode is not None:
               newlist.append(listNode.val)
               listNode = listNode.next
           return newlist[::-1]
   ```

   > Code_Java

   ```java
   public class Solution {
       Stack<Integer> stack = new Stack<>();
       while (listNode != null) {
           stack.push(listNode.val);
           listNode = listNode.next;
       }
       ArrayList<Integer> arrayList = new ArrayList<>();
       while (!stack.isEmpty()) {
           arrayList.add(stack.pop());
       }
       return arrayList;
       } 
   }
   ```

   

###### 2.3.4 树

> 面试题7 重建二叉树

1. [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal) (105)

   > Description

   Given preorder and inorder traversal of a tree, construct the binary tree.

   **Note:**
   You may assume that duplicates do not exist in the tree.

   For example, given

   ```
   preorder = [3,9,20,15,7]
   inorder = [9,3,15,20,7]
   ```

   Return the following binary tree:

   ```
       3
      / \
     9  20
       /  \
      15   7
   ```

   > Code_Python

   ```python
   class Solution:
       """
       主要解题思路，使用递归的方法
       1、前序遍历中第一个元素对应根节点（递归思路，每次都是根节点）
       2、根节点在中序遍历中每次将中序遍历平分为两分，左边和右边分别对应一个新的子树
       3、递归中止条件，当根节点再往下分子树，子树为空即结束递归
       """
       def buildTree(self, preorder, inorder):
           """
           :type preorder: List[int]
           :type inorder: List[int]
           :rtype: TreeNode
           """
           if (len(preorder) == 0 and len(inorder) == 0): return None
           root = TreeNode(preorder[0])
           pivot = inorder.index(preorder[0])
           root.left = self.buildTree(preorder[1:pivot + 1], inorder[0:pivot])
           root.right = self.buildTree(preorder[pivot + 1:], inorder[pivot + 1:])
           return root
   ```

   > Code_Java

   ```java
   class Solution {
       public TreeNode buildTree(int[] preorder, int[] inorder) {
           int start = 0;
           int end = preorder.length-1;
           return buildTreeStep(start,end,start,end,preorder,inorder);
       }
       //递归实现
       public TreeNode buildTreeStep(int preStart ,int preEnd ,int inStart,int inEnd,int[] preorder,int[] inorder){
           if (preEnd<preStart || inEnd<inStart) {
               return null;
           }
           TreeNode treeNode = new TreeNode(preorder[preStart]);
           int pivot  = getCurIndexInInOrder(inorder,inStart,inEnd,preorder[preStart]);
           treeNode.left = buildTreeStep(preStart+1,pivot+preStart,inStart,inStart+pivot-1,preorder,inorder);
           treeNode.right = buildTreeStep(preStart+pivot+1,preEnd,inStart+pivot+1,inEnd,preorder,inorder);
           return treeNode;
       }
       //get pivot index
       public int getCurIndexInInOrder(int[] inorder, int start, int end, int key){
           for(int i=start; i<=end; i++){
               if(inorder[i] == key) return i-start;
           }
           return -1;
       }
   }
   ```

2. [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal) (106)

   > Description

   Given inorder and postorder traversal of a tree, construct the binary tree.

   **Note:**
   You may assume that duplicates do not exist in the tree.

   For example, given

   ```
   inorder = [9,3,15,20,7]
   postorder = [9,15,7,20,3]
   ```

   Return the following binary tree:

   ```
       3
      / \
     9  20
       /  \
      15   7
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
       def buildTree(self, inorder, postorder):
           """
           :type inorder: List[int]
           :type postorder: List[int]
           :rtype: TreeNode
           """
           if not postorder or not inorder:return None
           root_index = postorder.pop()
           root = TreeNode(root_index)
           pivot = inorder.index(root_index)
           root.right=self.buildTree(inorder[pivot+1:],postorder[pivot:])
           root.left=self.buildTree(inorder[:pivot],postorder[:pivot])
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
       public TreeNode buildTree(int[] inorder, int[] postorder) {
           int start = 0;
           int end = postorder.length-1;
           return buildTreeStep(start,end,start,end,inorder,postorder);
       }
       //递归实现
       public TreeNode buildTreeStep(int inStart ,int inEnd ,int posStart,int posEnd,int[] inorder,int[] postorder){
           if (inEnd<inStart || posEnd<posStart) {
               return null;
           }
           TreeNode treeNode = new TreeNode(postorder[posEnd]);
           int pivot  = getCurIndexInInOrder(inorder,inStart,inEnd,postorder[posEnd]);
           treeNode.right = buildTreeStep(inStart+pivot+1,inEnd,posStart+pivot,posEnd-1,inorder,postorder);
           treeNode.left = buildTreeStep(inStart,inStart+pivot-1,posStart,posStart+pivot-1,inorder,postorder);
           return treeNode;
       }
       //get pivot index
       public int getCurIndexInInOrder(int[] inorder, int start, int end, int key){
           for(int i=start; i<=end; i++){
               if(inorder[i] == key) return i-start;
           }
           return -1;
       }
   }
   ```

> 面试题8 二叉树的下一个节点

1. LeetCode**无

   > Description

   给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。 

   > Code_Python

   ```python
   class TreeLinkNode:
       def __init__(self, x):
           self.val = x
           self.left = None
           self.right = None
           self.next = None
   
   class Solution1:
       def GetNext(self,pNode):
           if not pNode:
               return None
           if pNode.right:
               pNode = pNode.right
               while pNode.left is not None:
                   pNode = pNode.left
               return pNode
           
           while pNode.next is not None:
               if pNode.next.left is pNode:
                   return pNode.next
               pNode == pNode.next
           return None
   ```

   > Code_Java

   ```java
   public class Solution {
       /**
        * 如果有右子树，则找右子树的最左节点
        * 没右子树，则找第一个当前节点是父节点左孩子的节点
       */
       TreeLinkNode GetNext(TreeLinkNode node)
       {
           if(node==null) return null;
           if(node.right!=null){    
               node = node.right;
               while(node.left!=null) {
                   node = node.left;
               }
               return node;
           }
           while(node.next!=null){ 
               if(node.next.left==node) {
                   return node.next;
               }
               node = node.next;
           }
           return null;   //退到了根节点仍没找到，则返回null
       }
   }
   ```

###### 2.3.5 栈和队列

> 面试题9 用两个栈实现队列

1. [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks) (232)

   > Description

   Implement the following operations of a queue using stacks.

   - push(x) -- Push element x to the back of queue.
   - pop() -- Removes the element from in front of queue.
   - peek() -- Get the front element.
   - empty() -- Return whether the queue is empty.

   **Example:**

   ```
   MyQueue queue = new MyQueue();
   
   queue.push(1);
   queue.push(2);  
   queue.peek();  // returns 1
   queue.pop();   // returns 1
   queue.empty(); // returns false
   ```

   **Notes:**

   - You must use *only* standard operations of a stack -- which means only `push to top`, `peek/pop from top`, `size`, and `is empty`operations are valid.
   - Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
   - You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

   > Code_Python

   ```python
   
   ```

   > Code_Java

   ```java
   
   ```

   

2. [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues) (225)

   > Description

   

###### 2.4.1 递归和循环

> 面试题10 斐波那契(Fibonacci sequence )数列

