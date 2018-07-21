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
   class MyQueue:
   
       def __init__(self):
           """
           Initialize your data structure here.
           """
           self.in_stack = []
           self.out_stack = []
   
       def push(self, x):
           """
           Push element x to the back of queue.
           :type x: int
           :rtype: void
           """
           self.in_stack.append(x)
   
       def pop(self):
           """
           Removes the element from in front of queue and returns that element.
           :rtype: int
           """
           x = self.peek()
           self.out_stack.pop()   
           return x
   
       def peek(self):
           """
           Get the front element.
           :rtype: int
           """
           if not self.out_stack:
               while self.in_stack:
                   self.out_stack.append(self.in_stack.pop())
           return self.out_stack[-1]
   
       def empty(self):
           """
           Returns whether the queue is empty.
           :rtype: bool
           """
           return (not self.in_stack and not self.out_stack)
           
   
   
   # Your MyQueue object will be instantiated and called as such:
   # obj = MyQueue()
   # obj.push(x)
   # param_2 = obj.pop()
   # param_3 = obj.peek()
   # param_4 = obj.empty()
   ```

   > Code_Java

   ```java
   class MyQueue {
       Stack<Integer> inStack;
       Stack<Integer> outStack;
       /** Initialize your data structure here. */
       public MyQueue() {
           inStack = new Stack<>();
           outStack = new Stack<>();
       }
   
       /** Push element x to the back of queue. */
       public void push(int x) {
           if (inStack.isEmpty()){
               inStack.push(x);
           }else {
               while (!inStack.isEmpty()){
                   outStack.push(inStack.pop());
               }
               inStack.push(x);
               while (!outStack.isEmpty()) {
                   inStack.push(outStack.pop());
               }
           }
       }
   
       /** Removes the element from in front of queue and returns that element. */
       public int pop() {
           return inStack.pop();
       }
   
       /** Get the front element. */
       public int peek() {
           return inStack.peek();
       }
   
       /** Returns whether the queue is empty. */
       public boolean empty() {
           return (inStack.isEmpty());
       }
   }
   
   /**
    * Your MyQueue object will be instantiated and called as such:
    * MyQueue obj = new MyQueue();
    * obj.push(x);
    * int param_2 = obj.pop();
    * int param_3 = obj.peek();
    * boolean param_4 = obj.empty();
    */
   ```

   

2. [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues) (225)

   > Description

   Implement the following operations of a stack using queues.

   - push(x) -- Push element x onto stack.
   - pop() -- Removes the element on top of the stack.
   - top() -- Get the top element.
   - empty() -- Return whether the stack is empty.

   **Example:**

   ```
   MyStack stack = new MyStack();
   
   stack.push(1);
   stack.push(2);  
   stack.top();   // returns 2
   stack.pop();   // returns 2
   stack.empty(); // returns false
   ```

   **Notes:**

   - You must use *only* standard operations of a queue -- which means only `push to back`, `peek/pop from front`, `size`, and `is empty` operations are valid.
   - Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
   - You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

   > Code_Python

   ```python
   class MyStack:
   
       def __init__(self):
           """
           Initialize your data structure here.
           """
           self.queue_1 = []
           self.queue_2 = []
       def push(self, x):
           """
           Push element x onto stack.
           :type x: int
           :rtype: void
           """
           if self.queue_1:
               self.queue_1.append(x)
           else:
               self.queue_2.append(x)
   
       def pop(self):
           """
           Removes the element on top of the stack and returns that element.
           :rtype: int
           """
           if self.queue_1:
               for q in range(0,len(self.queue_1)-1):
                   self.queue_2.append(self.queue_1.pop(0))
               return self.queue_1.pop(0)
           else:
               for q in range(0,len(self.queue_2)-1):
                   self.queue_1.append(self.queue_2.pop(0))
               return self.queue_2.pop(0)
   
       def top(self):
           """
           Get the top element.
           :rtype: int
           """
           if self.queue_1:
               for q in range(0,len(self.queue_1)-1):
                   self.queue_2.append(self.queue_1.pop(0))
               temp = self.queue_1.pop(0)
               self.queue_2.append(temp)
               return temp
           else:
               for q in range(0,len(self.queue_2)-1):
                   self.queue_1.append(self.queue_2.pop(0))
               temp = self.queue_2.pop(0)
               self.queue_1.append(temp)
               return temp
       def empty(self):
           """
           Returns whether the stack is empty.
           :rtype: bool
           """
           return (not self.queue_1 and not self.queue_2)
   
   # Your MyStack object will be instantiated and called as such:
   # obj = MyStack()
   # obj.push(x)
   # param_2 = obj.pop()
   # param_3 = obj.top()
   # param_4 = obj.empty()
   ```

   > Code_Java

   ```java
   class MyStack {
   
       //Queue使用时要尽量避免Collection的add()和remove()方法，而是要使用offer()来加入元素，使用poll()来获取并移出元素。
       Queue<Integer> queue1;
       Queue<Integer> queue2;
       /** Initialize your data structure here. */
       public MyStack() {
           queue1 = new LinkedList<>();
           queue2 = new LinkedList<>();
       }
   
       /** Push element x onto stack. */
       public void push(int x) {
           if (!queue1.isEmpty()){
               queue1.offer(x);
           }else {
               queue2.offer(x);
           }
       }
   
       /** Removes the element on top of the stack and returns that element. */
       public int pop() {
           if (!queue1.isEmpty()){
               int length = queue1.size();
               for (int i = 0; i < length-1; i++) {
                   queue2.offer(queue1.poll());
               }
               return queue1.poll();
           }else {
               int length = queue2.size();
               for (int i = 0; i < length-1; i++) {
                   queue1.offer(queue2.poll());
               }
               return queue2.poll();
           }
       }
   
       /** Get the top element. */
       public int top() {
           
           if (!queue1.isEmpty()){
               int length = queue1.size();
               for (int i = 0; i < length-1; i++) {
                   queue2.offer(queue1.poll());
               }
               int temp = queue1.poll();
               queue2.offer(temp);
               return temp;
           }else {
               int length = queue2.size();
               for (int i = 0; i < length-1; i++) {
                   queue1.offer(queue2.poll());
               }
               int temp = queue2.poll();
               queue1.offer(temp);
               return temp;
           }
   
       }
   
       /** Returns whether the stack is empty. */
       public boolean empty() {
           return (queue1.size()==0 && queue2.size()==0);
       }
   }
   
   /**
    * Your MyStack object will be instantiated and called as such:
    * MyStack obj = new MyStack();
    * obj.push(x);
    * int param_2 = obj.pop();
    * int param_3 = obj.top();
    * boolean param_4 = obj.empty();
    */
   ```

###### 2.4.1 递归和循环

> 面试题10 斐波那契(Fibonacci sequence )数列

1. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs) (70)

   > Description

   You are climbing a stair case. It takes *n* steps to reach to the top.

   Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

   **Note:** Given *n* will be a positive integer.

   **Example 1:**

   ```
   Input: 2
   Output: 2
   Explanation: There are two ways to climb to the top.
   1. 1 step + 1 step
   2. 2 steps
   ```

   **Example 2:**

   ```
   Input: 3
   Output: 3
   Explanation: There are three ways to climb to the top.
   1. 1 step + 1 step + 1 step
   2. 1 step + 2 steps
   3. 2 steps + 1 step
   ```

   > Code_Python

   ```python
   class Solution:
       '''
       递归实现，时间复杂度过高，使用循环实现，循环实现思路，
       通过f(0)+f(1)=f(2)计算f(3)=f(2)+f(1) ...
       '''
       def climbStairs(self, n):
           """
           :type n: int
           :rtype: int
           """
           if n < 2: return n
           fibNMinusOne = 1
           fibNMinusTwo = 0
           fibN = 0
           for _ in range(n):
               fibN = fibNMinusOne + fibNMinusTwo
               fibNMinusTwo = fibNMinusOne
               fibNMinusOne = fibN
           return fibN
   ```

   > Code_Java

   ```java
   class Solution {
       public int climbStairs(int n) {
           if (n<2){
               return n;
           }
           int fibNMinusOne = 1;
           int fibNMinusTwo = 0;
           int fibN = 0;
           for (int i=1;i<=n;++i){
               fibN = fibNMinusOne+fibNMinusTwo;
               fibNMinusTwo = fibNMinusOne;
               fibNMinusOne=fibN;
           }
           return fibN;
       }
   }
   ```

###### 2.4.2 查找和排序

> 面试题11：旋转数组的最小数字

1. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array) (153)

   > Description

   Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

   (i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

   Find the minimum element.

   You may assume no duplicate exists in the array.

   **Example 1:**

   ```
   Input: [3,4,5,1,2] 
   Output: 1
   ```

   **Example 2:**

   ```
   Input: [4,5,6,7,0,1,2]
   Output: 0
   ```

   > Code_Python

   ```python
   def findMin(nums):
       """
       :type nums: List[int]
       :rtype: int
       """
       if not nums:
           return nums
       start = 0
       end = len(nums) - 1
       while start <= end:
           mid = int((start + end) / 2)
           if nums[start] <= nums[end]:
               return nums[start]
           elif nums[start] > nums[mid]:
               end = mid
           else:
               start = mid + 1
   ```

   > Code_Java

   ```java
   class Solution {
       public int findMin(int[] nums) {
            int start = 0;
           int end = nums.length - 1;
           int mid = 0;
           while (start <= end) {
               mid = (start+end)/2;
               if (nums[start] <= nums[end]) {
                   return nums[start];
               } else if (nums[start] > nums[mid]) {
                   end = mid;
               } else {
                   start = mid + 1;
               }
           }
           return -1;
       }
   }
   ```

   

2. [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii) (154)

   > Description

   Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

   (i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

   Find the minimum element.

   The array may contain duplicates.

   **Example 1:**

   ```
   Input: [1,3,5]
   Output: 1
   ```

   **Example 2:**

   ```
   Input: [2,2,2,0,1]
   Output: 0
   ```

   **Note:**

   - This is a follow up problem to [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/).
   - Would allow duplicates affect the run-time complexity? How and why?

   > Code_Python

   ```python
   class Solution:
       def findMin(self, nums):
           """
           :type nums: List[int]
           :rtype: int
           """
           if not nums:
               return nums
           start = 0
           end = len(nums) - 1
           while start <= end:
               mid = int((start + end) / 2)
               # 判断[3,1,3]情况
               if(nums[start]==nums[end]):
                   for i in range(start,end):
                       if nums[i]<nums[start]:
                           return nums[i]
                   return nums[start]
               if (nums[start] <nums[end]) :
                   return nums[start]
               elif nums[start] > nums[mid]:
                   end = mid
               else:
                   start = mid+1
   ```

   > Code_Java

   ```java
   class Solution {
       public int findMin(int[] nums) {
           int start = 0;
           int end = nums.length - 1;
           int mid = 0;
           while (start <= end) {
               mid = (start + end) / 2;
               if (nums[start]==nums[end]){
                   for (int i = start ;i<end;++i){
                       if (nums[i]<nums[start]){
                           return nums[i];
                       }
                   }
                   return nums[start];
               }
               if (nums[start] < nums[end]) {
                   return nums[start];
               } else if (nums[start] > nums[mid]) {
                   end = mid;
               } else {
                   start = mid + 1;
               }
           }
           return -1;
       }
   }
   ```

###### 2.4.3 回溯法

> 面试题12：矩阵中的路径

1. [Unique Paths](https://leetcode.com/problems/unique-paths) (62)

   > Description

   A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

   The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

   How many possible unique paths are there?

   ![img](https://leetcode.com/static/images/problemset/robot_maze.png)
   Above is a 7 x 3 grid. How many possible unique paths are there?

   **Note:** *m* and *n* will be at most 100.

   **Example 1:**

   ```
   Input: m = 3, n = 2
   Output: 3
   Explanation:
   From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
   1. Right -> Right -> Down
   2. Right -> Down -> Right
   3. Down -> Right -> Right
   ```

   **Example 2:**

   ```
   Input: m = 7, n = 3
   Output: 28
   ```

   > Code_Python

   ```python
   class Solution:
       def uniquePaths(self, m, n):
           """
           :type m: int
           :type n: int
           :rtype: int
           """
           map_all = []
           # 第一步先给边界点赋值为1
           for col in range(m):
               row_map = []
               for row in range(n):
                   if row == 0 or col == 0:
                       temp = 1
                   else:
                       temp = 0
                   row_map.append(temp)
               map_all.append(row_map)
           # 计算夹角值，等于左边和上边值得和
           for i in range(1,m):
               for j in range(1,n):
                   map_all[i][j] = map_all[i][j - 1] + map_all[i - 1][j]
           return map_all[m-1][n-1]
   ```

   > Code_Java

   ```java
   class Solution {
       public int uniquePaths(int m, int n) {
           int[][] map = new int[m][n];
           for (int i = 0; i < m; i++) {
               map[i][0] = 1;
           }
           for (int i = 0; i < n; i++) {
               map[0][i] = 1;
           }
           for (int i = 1; i < m; i++) {
               for (int j = 1; j < n; j++) {
                   map[i][j] = map[i - 1][j] + map[i][j - 1];
               }
           }
           return map[m - 1][n - 1];
       }
   }
   ```

2. [Unique Paths II](https://leetcode.com/problems/unique-paths-ii) (63)

   > Description

   A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

   The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

   Now consider if some obstacles are added to the grids. How many unique paths would there be?

   ![img](https://leetcode.com/static/images/problemset/robot_maze.png)

   An obstacle and empty space is marked as `1` and `0` respectively in the grid.

   **Note:** *m* and *n* will be at most 100.

   **Example 1:**

   ```
   Input:
   [
     [0,0,0],
     [0,1,0],
     [0,0,0]
   ]
   Output: 2
   Explanation:
   There is one obstacle in the middle of the 3x3 grid above.
   There are two ways to reach the bottom-right corner:
   1. Right -> Right -> Down -> Down
   2. Down -> Down -> Right -> Right
   ```

   > Code_Python

   ```python
   class Solution:
       def uniquePathsWithObstacles(self, obstacleGrid):
           """
           :type obstacleGrid: List[List[int]]
           :rtype: int
           """
           m = len(obstacleGrid)
           n = len(obstacleGrid[0])
           dp = [[0]*n]*m
           for i in range(m):
               for j in range(n):
                   if obstacleGrid[i][j] == 0:
                       if i==0 and j==0:
                           dp[i][j] = 1
                       elif i==0:
                           dp[i][j] = dp[i][j-1]
                       elif j==0:
                           dp[i][j] = dp[i-1][j]
                       else:
                           dp[i][j] = dp[i-1][j]+dp[i][j-1]
                   else:
                       dp[i][j] = 0
           return dp[m-1][n-1]
   ```

   > Code_Java

   ```java
   class Solution {
       public int uniquePathsWithObstacles(int[][] obstacleGrid) {
           int m = obstacleGrid.length;
           int n = obstacleGrid[0].length;
           int[][] map= new int[m][n];
           if(obstacleGrid[0][0]==0){
               map[0][0]=1;
           }else{
               map[0][0]=0;
           }
           
           for(int i = 1;i<m;i++){
               map[i][0]=(obstacleGrid[i][0]==1)? 0:map[i-1][0];
           }
           for(int j = 1;j<n;j++){
               map[0][j] =(obstacleGrid[0][j]==1)? 0: map[0][j-1];
           }
           for(int i = 1;i<m;i++){
               for(int j =1;j<n;j++){
                   map[i][j] =(obstacleGrid[i][j]==1)? 0: map[i-1][j]+map[i][j-1];
               }
           }
           return map[m-1][n-1];
       }
   
   }
   ```

   

> 面试题13：机器人的运动范围

1. LeetCode**无

   > Description

   地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

   > Code_Python

   ```python
   class Solution:
       #计算数字之和
       def getDigitalSum(self, value):
           sum=0
           while value>=1:
               sum+=value%10
               value=int(value/10)
           return sum
   
       def movingCountCore(self, threshold, rows, cols, row, col, visited):
           count = 0
           # 判断是否满足要求，即在边界范围内且未到达过并且数字之和合理
           if row >= 0 and row < rows and col >= 0 and col < cols and visited[row*cols+col] and (self.getDigitalSum(col) + self.getDigitalSum(row) <= threshold):
               visited[row * cols + col] = 0
               count=1+self.movingCountCore(threshold,rows,cols,row-1,col,visited)+self.movingCountCore(threshold,rows,cols,row,col-1,visited)+self.movingCountCore(threshold,rows,cols,row+1,col,visited)+self.movingCountCore(threshold,rows,cols,row,col+1,visited)
           return count
   
       def movingCount(self, threshold, rows, cols):
           if threshold <0 or rows<0 or cols<0:return 0
           # 0 false 1 true
           visited = [1]*(rows*cols)
           return self.movingCountCore(threshold,rows,cols,0,0,visited)
   ```

   > Code_Java

   ```java
   public class Solution {
       public int movingCount(int threshold, int rows, int cols) {
           if (threshold < 0) {
               return 0;
           }
           boolean[] visited = new boolean[rows * cols];
           int count = movingCountCore(threshold, rows, cols, 0, 0, visited);
           return count;
       }
   
       private int movingCountCore(int threshold, int rows, int cols, int row, int col, boolean[] visited) {
           int count = 0;
           //判断是否满足要求，即在边界范围内且未到达过并且数字之和合理
           if (row >= 0 && row < rows && col >= cols && col < cols && visited[row * cols + col] == false && getDigitalSum(col) + getDigitalSum(row) <= threshold) {
               visited[row * cols + col] = true;
               count = 1 + movingCountCore(threshold, rows, cols, row + 1, col, visited) + movingCountCore(threshold, rows, cols, row - 1, col, visited) + movingCountCore(threshold, rows, cols, row, col + 1, visited)
                       + movingCountCore(threshold, rows, cols, row, col - 1, visited);
           }
           return count;
       }
   
       /**
        * 计算数字各个位之和
        *
        * @param value 需要求和的数字
        * @return 返回数字之和
        */
       private int getDigitalSum(int value) {
           int sum = 0;
           while (value != 0) {
               sum += value % 10;
               value = value / 10;
           }
           return sum;
       }
   
   }
   ```

###### 2.4.4 动态规划与贪婪算法

> 面试题14：剪绳子

1. LeetCode**无

   > Description

   给你一根长度为n的绳子，请把绳子剪成m段（m , n ）都是正整数，（n>1&m>1）

   每段绳子的长度为k[0],k[1],k[2],...,k[m]。请问k[0]*k[1]*k[2]*...*k[m]的最大值。

   例如绳子是长度为8，我们把它剪成的长度分别为2,3,3的三段，此时得到的最大的乘积是18。

   > Code_Python

   ```python
   class Solution:
       def maxProduct(self, length):
           if length < 2:
               return 0
           if length < 4:
               return length
           # 构造一个n+1的列表因为0不用所以为n+1,暂存使用
           products = [0] * (length+1)
           products[0], products[1], products[2], products[3] = 0, 1, 2, 3
           for i in range(4, length+1):
               max = 0
               for j in range(1, int(i / 2)+1):
                   # 在剪第一刀的时候，又n-2中可能的选择，也就是剪出来的绳子的可能长度分别为1，2，...n，因此f(n)=max(f(i)*f(n-i))
                   product = products[j] * products[i - j]
                   if max < product:
                       max = product
                   products[i] = max
           return products[-1]
   ```

   > Code_Java

   ```java
   public class Solution {
       public static int maxProduct(int length){
           if (length < 2) {
               return 0;
           }
           if (length < 4) {
               return length;
           }
           int[] products =new int[length+1];
           products[0] = 0;
           products[1] = 1;
           products[2] = 2;
           products[3] = 3;
           int max = 0 ;
           for (int i = 4; i <= length; i++) {
               max =0;
               for (int j = 1; j <= i/2; j++) {
                   int product = products[j] * products[i - j];
   
                   if (max  < product) {
                       max = product;
                   }
                   products[i]=max;
               }
           }
           return products[length];
   
       } 
   ```

   

###### 2.4.5 位运算

> 面试题15：二进制中1的个数

1. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits) (191)

   > Description

   Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

   **Example 1:**

   ```
   Input: 11
   Output: 3
   Explanation: Integer 11 has binary representation 00000000000000000000000000001011 
   ```

   **Example 2:**

   ```
   Input: 128
   Output: 1
   Explanation: Integer 128 has binary representation 00000000000000000000000010000000
   ```

   > Code_Python

   ```python
   class Solution(object):
       def hammingWeight(self, n):
           """
           :type n: int
           :rtype: int
           """
           count = 0
           while n:
               count+=1
               n=n&(n-1)
           return count
   ```

   > Code_Java

   ```java
   public class Solution {
       // you need to treat n as an unsigned value
       public int hammingWeight(int n) {
           int total = 0;
           while (n != 0) {
               total++;
               n=n&(n-1);
           }
           return total;
       }
   }
   ```

###### 3.3 代码的完整性

> 面试题16：数值的整数次方

1. [Pow(x, n)](https://leetcode.com/problems/powx-n) (50) 

   > Description

   Implement [pow(*x*, *n*)](http://www.cplusplus.com/reference/valarray/pow/), which calculates *x* raised to the power *n* (xn).

   **Example 1:**

   ```
   Input: 2.00000, 10
   Output: 1024.00000
   ```

   **Example 2:**

   ```
   Input: 2.10000, 3
   Output: 9.26100
   ```

   **Example 3:**

   ```
   Input: 2.00000, -2
   Output: 0.25000
   Explanation: 2-2 = 1/22 = 1/4 = 0.25
   ```

   **Note:**

   - -100.0 < *x* < 100.0
   - *n* is a 32-bit signed integer, within the range [−231, 231 − 1]

   > Code_Python

   ```python
   class Solution(object):
       def myPow(self, x, n):
           """
           :type x: float
           :type n: int
           :rtype: float
           """
           if n<0:x,n=1/x,-n
           res = 1
           while n>0:
               if n%2!=0:
                   res *=x
               x*=x
               n=n//2
           return res
           
   ```

   > Code_Java

   ```java
   class Solution {
       public double myPow(double x, int n) {
           long N = n;
           if (N<0){
               x=1/x;
               N = -N;
           }
           double result = 1;
           while (N > 0) {
               if (N % 2 == 1) {
                   result = result*x;
               }
               N = N/2;
               x=x*x;
           }
           return result;
       }
   }
   ```

   

 

 