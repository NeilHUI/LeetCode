### 剑指offer——LeetCode~16-26

##### 3 高质量的代码

###### 3.3 代码的完整性

> 面试题16 数值的整数次方

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


> 面试题17：打印从1到最大的n位数

1. [Nth Digit](https://leetcode.com/problems/nth-digit) (400)

   Find the *n*th digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...

   **Note:**
   *n* is positive and will fit within the range of a 32-bit signed integer (*n* < 231).

   **Example 1:**

   ```
   Input:
   3
   
   Output:
   3
   ```

   **Example 2:**

   ```
   Input:
   11
   
   Output:
   0
   
   Explanation:
   The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.
   ```

   > Code_Python

   ```python
   class Solution(object):
       def findNthDigit(self, n):
           """
           :type n: int
           :rtype: int
           """
           base, digit, temp = 9, 1, 1
           while n > base * digit:
               n -= base * digit
               digit += 1
               temp += base
               base *= 10
           return int(str(temp + (n - 1) // digit)[(n - 1) % digit])
   
   
   ```

   > Code_Java

   ```java
   class Solution {
       public int findNthDigit(int n) {
           //当n为2147483647即base*time 会大于int最大空间2^23-1，所以使用long大小为2^63-1类型
           long base = 9;
           int temp = 1;
           int time = 1;
           while (n > base * time) {
               n -= base * time;
               temp += base;
               time += 1;
               base *= 10;
           }
           return ((temp + (n - 1) / time) + "").charAt((n - 1) % time) - '0';
       }
   }
   ```

> 面试题18：删除链表的节点

1. [Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list) (237)

   > Description

   Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

   Given linked list -- head = [4,5,1,9], which looks like following:

   ```
       4 -> 5 -> 1 -> 9
   ```

   **Example 1:**

   ```
   Input: head = [4,5,1,9], node = 5
   Output: [4,1,9]
   Explanation: You are given the second node with value 5, the linked list
                should become 4 -> 1 -> 9 after calling your function.
   ```

   **Example 2:**

   ```
   Input: head = [4,5,1,9], node = 1
   Output: [4,5,9]
   Explanation: You are given the third node with value 1, the linked list
                should become 4 -> 5 -> 9 after calling your function.
   ```

   **Note:**

   - The linked list will have at least two elements.
   - All of the nodes' values will be unique.
   - The given node will not be the tail and it will always be a valid node of the linked list.
   - Do not return anything from your function.

   > Code_Python

   ```python
   # Definition for singly-linked list.
   # class ListNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   
   class Solution:
       def deleteNode(self, node):
           """
           :type node: ListNode
           :rtype: void Do not return anything, modify node in-place instead.
           """
           if node.next:
               node.val,node.next = node.next.val,node.next.next
   ```

   > Code_Java

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) { val = x; }
    * }
    */
   class Solution {
       public void deleteNode(ListNode node) {
           if(node.next!=null){
               node.val = node.next.val;
               node.next = node.next.next;
           }
           
       }
   }
   ```

> 面试题19：正则表达式匹配

1. [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching) (10)

   > Description

   Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

   ```
   '.' Matches any single character.
   '*' Matches zero or more of the preceding element.
   ```

   The matching should cover the **entire** input string (not partial).

   **Note:**

   - `s` could be empty and contains only lowercase letters `a-z`.
   - `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

   **Example 1:**

   ```
   Input:
   s = "aa"
   p = "a"
   Output: false
   Explanation: "a" does not match the entire string "aa".
   ```

   **Example 2:**

   ```
   Input:
   s = "aa"
   p = "a*"
   Output: true
   Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
   ```

   **Example 3:**

   ```
   Input:
   s = "ab"
   p = ".*"
   Output: true
   Explanation: ".*" means "zero or more (*) of any character (.)".
   ```

   **Example 4:**

   ```
   Input:
   s = "aab"
   p = "c*a*b"
   Output: true
   Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
   ```

   **Example 5:**

   ```
   Input:
   s = "mississippi"
   p = "mis*is*p*."
   Output: false
   ```

   > Code_Python

   ```python
   class Solution(object):
       # 动态规划效率会高
       def isMatch(self, s, p):
           """
           :type s: str
           :type p: str
           :rtype: bool
           """
           if not p:return s == ""
           # 判断匹配字符长度为1时，两者的第一个元素是否相同，或匹配.
           if len(p) == 1:return len(s) == 1 and (s[0] ==p[0] or p[0] =='.')
           # 如果第二个元素不为*，当匹配字符串不为空的时候，判断两个元素是否相同或者p是否为.，然后递归新的字符串进行判断
           if p[1]!="*":
               if not s:return False
               return (s[0] == p[0] or p[0]=='.') and self.isMatch(s[1:],p[1:])
           while s and (s[0]==p[0] or p[0] == '.'):
               # 用于跳出函数，当s循环到和*不匹配的时候，则开始去匹配p[2:]之后的规则
               if self.isMatch(s,p[2:]):return True
               # 当匹配字符串和匹配规则*都能匹配的时候，去掉s中第一个字符成为新的匹配字符串，循环
               s = s[1:]
           # 假如第一个字符和匹配规则不匹配，则*代表0然后去判断之后的是否匹配
           return self.isMatch(s,p[2:])
   ```

   > Code_Java

   ```java
   class Solution {
       public boolean isMatch(String s, String p) {
           if (p.isEmpty()) return s.isEmpty();
           boolean first_match = (!s.isEmpty() && (p.charAt(0) == s.charAt(0) || p.charAt(0) == '.'));
           if (p.length() >= 2 && p.charAt(1) == '*'){
               return (isMatch(s, p.substring(2)) || (first_match && isMatch(s.substring(1), p)));
           } else {
               return first_match && isMatch(s.substring(1), p.substring(1));
           }
       }
   }
   ```

> 面试题20：表示数值的字符串

1. [Valid Number](https://leetcode.com/problems/valid-number) (65)

   > Description

   Validate if a given string is numeric.

   Some examples:
   `"0"` => `true`
   `" 0.1 "` => `true`
   `"abc"` => `false`
   `"1 a"` => `false`
   `"2e10"` => `true`

   **Note:** It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one.

   **Update (2015-02-10):**
   The signature of the `C++` function had been updated. If you still see your function signature accepts a `const char *` argument, please click the reload button to reset your code definition.

   > Code_Python

   ```python
   class Solution:
        def isNumber(self, s):
           """
           :type s: str
           :rtype: bool
           """
           s = s.strip()
           length = len(s)
           index = 0
           # Deal with symbol
           if index < length and (s[index] == '+' or s[index] == '-'):
               index += 1
           is_normal = False
           is_exp = True
           # Deal with digits in the front
           while index < length and s[index].isdigit():
               is_normal = True
               index += 1
           # Deal with dot ant digits behind it
           if index < length and s[index] == '.':
               index += 1
               while index < length and s[index].isdigit():
                   is_normal = True
                   index += 1
           # Deal with 'e' and number behind it
           if is_normal and index < length and (s[index] == 'e' or s[index] == 'E'):
               index += 1
               is_exp = False
               if index < length and (s[index] == '+' or s[index] == '-'):
                   index += 1
               while index < length and s[index].isdigit():
                   index += 1
                   is_exp = True
           # Return true only deal with all the characters and the part in front of and behind 'e' are all ok
           return is_normal and is_exp and index == length
   ```

   > Code_Java

   ```java
   
   ```

> 面试题21：调整数组顺序使奇数位于偶数前面

1. LeetCode**无

   > Description

   输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

   > Code_Python

   ```python
   class Solution:
       def reOrderArray(self, array):
           # write code here
           if not len(array): return []
           index_s = 0
           index_e = len(array) - 1
           result = [0] * (index_e + 1)
           list_odd = []
           for num in array[::-1]:
               if num % 2 == 0:
                   result[index_e] = num
                   index_e -= 1
               else:
                   list_odd.append(num)
                   index_s += 1
           for i in range(index_s):
               result[i]=list_odd.pop()
           return result
   ```

   > Code_Java

   ```java
   private static void ReorderArray(int[] array){
       //不考虑位置顺序变化
       int left = 0;
       int right = array.length-1;
       while(left<right){
           while(left<right && (array[left]&1)==1)//使用位运算判断奇偶
               left++;//arr[left]为奇数，自增直至为偶数为止
           while(left<right && (array[right]&1)!=1)
               right--;//arr[right]为偶数，自减直至为奇数为止
           //arr[left]为偶数，arr[right]为奇数，交换
           if(left<right){
               int temp = array[left];
               array[left] = array[right];
               array[right] = temp;
           }
       }
   }
   ```

###### 3.4代码的鲁棒性

> 面试题22：链表中倒数第k个节点

1. LeetCode**无

   > Description

   输入一个链表，输出该链表中倒数第k个结点。 

   > Code_Python

   ```python
   # -*- coding:utf-8 -*-
   # class ListNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   class Solution:
       def FindKthToTail(self, head, k):
           if head<1 or head==None:return None
           else:
               p=head
               k_listNode = head
               while p:
                   count+=1
                   if count>k:
                       k_listNode=k_listNode.next
                   p=p.next
               if count<k:
                   return None
             return k_listNode
   ```

   > Code_Java

   ```java
   public class Solution {
       public ListNode FindKthToTail(ListNode head,int k){
           if(head == null || k<=0){
               return null;
           }
           ListNode nodeP = head;
           ListNode nodeL = head;
           for(int i=0;i<k-1;i++){
               if(nodeP.next!==null){
                   nodeP == nodeP.next;
               }else{
                   return null;
               }
           }
           while(Nodep.next!=null){
               Nodep = nodeP.next;
               nodeL = nodeL.next;
           }
           return nodeL;
       }
   ```

> 面试题23：链表中环的入口节点

1. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle) (141)

   Given a linked list, determine if it has a cycle in it.

   Follow up:
   Can you solve it without using extra space?

   > Code_Python

   ```python
   # Definition for singly-linked list.
   # class ListNode(object):
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   
   class Solution(object):
       def hasCycle(self, head):
           """
           :type head: ListNode
           :rtype: bool
           """
           if not head or head.next==None:
               return False
           pre = head
           suf = head
           while pre.next!=None:
               if pre.next.next == None:
                   return False
               pre = pre.next.next
               suf = suf.next
               if pre == suf:
                   return True
           return False
   ```

   > Code_Java

   ```java
   /**
    * Definition for singly-linked list.
    * class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) {
    *         val = x;
    *         next = null;
    *     }
    * }
    */
   public class Solution {
       public boolean hasCycle(ListNode head) {
           if (head==null){
               return false;
           }
           ListNode firstNode = head;
           ListNode secondNode = head;
           while (firstNode!=null && secondNode!=null && firstNode.next!=null){
               firstNode=firstNode.next.next;
               secondNode=secondNode.next;
               if(firstNode==secondNode){
                   return true;
               }
           }
           return false;
       }
   }
   ```

2. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii) (142)

   Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

   **Note:** Do not modify the linked list.

   > Code_Python

   ```python
   # Definition for singly-linked list.
   # class ListNode(object):
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   
   class Solution(object):
       def detectCycle(self, head):
           """
           :type head: ListNode
           :rtype: ListNode
           """
           if not head:return None
           first_node,second_node = head,head
           while first_node and second_node and first_node.next:
               first_node = first_node.next.next
               second_node = second_node.next
               if first_node == second_node:
                   result_node = head
                   while result_node != second_node:
                           second_node = second_node.next
                           result_node = result_node.next
                   return result_node    
           return None
               
               
           
   ```

   > Code_Java

   ```java
   /**
    * Definition for singly-linked list.
    * class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) {
    *         val = x;
    *         next = null;
    *     }
    * }
    */
   public class Solution {
       //
       public ListNode detectCycle(ListNode head) {
           if(head == null){
               return null;
           }
           //前两个节点目的是判断是否有环
           ListNode first = head;
           ListNode second = head;
           //当有环的时候，第一个节点和result_node节点同时往后走，相遇的时候即为环节点
           ListNode result_node = head;
           while (first!=null && second!=null && first.next!=null){
               first = first.next.next;
               second = second.next;
               if(first==second){
                   while(first != result_node){
                       first = first.next;
                       result_node = result_node.next;
                   }
                   return result_node;
               }
           }
           return null;
       }
   }
   ```

> 面试题24：反转链表

1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list) (206)

   > Description

   Reverse a singly linked list.

   **Example:**

   ```
   Input: 1->2->3->4->5->NULL
   Output: 5->4->3->2->1->NULL
   ```

   > Code_Python

   ```python
   # Definition for singly-linked list.
   # class ListNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   
   class Solution:
       def reverseList(self, head):
           """
           :type head: ListNode
           :rtype: ListNode
           """
           # 如果节点为空
           if not head:return None
           p_node = head
           p_reversed_head = None
           p_prev = None
           while p_node:
               p_next = p_node.next
               # 节点不为空则开始替换当前节点
               if p_node:
                   p_reversed_head = p_node
               p_node.next = p_prev
               p_prev = p_node
               p_node = p_next
               
           return p_reversed_head
   ```

   > Code_Java

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) { val = x; }
    * }
    */
   class Solution {
       public ListNode reverseList(ListNode head) {
       	ListNode pre = null;
           while(head != null){
               ListNode cur = head;
               head = head.next;
               cur.next = pre;
               pre = cur;
           }
           return pre;
       }
   }
   ```

> 面试题25：合并两个排序的链表

1. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists) (21)

   Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

   **Example:**

   ```
   Input: 1->2->4, 1->3->4
   Output: 1->1->2->3->4->4 
   ```

   > Code_Python

   ```python
   # Definition for singly-linked list.
   # class ListNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   
   class Solution:
       def mergeTwoLists(self, l1, l2):
           """
           :type l1: ListNode
           :type l2: ListNode
           :rtype: ListNode
           """
           if not l1:return l2
           if not l2:return l1
           if l1.val>l2.val:
               head =l2
               l2 = l2.next
           else:
               head = l1
               l1 = l1.next
           result = head
           while l1 and l2:
               if l1.val>l2.val:
                   head.next = l2
                   l2 = l2.next
                   head = head.next
               else:
                   head.next = l1
                   l1 = l1.next
                   head = head.next
           if l1:head.next = l1
           if l2:head.next = l2
           return result
   ```

   > Code_Java

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) { val = x; }
    * }
    */
   class Solution {
       public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
           if(l1==null){return l2;}
           if(l2==null){return l1;}
           ListNode mergeNode = null;
           if(l1.val>l2.val){
               mergeNode = l2;
               l2 = l2.next;
           }else{
               mergeNode = l1;
               l1 = l1.next;
           }
           ListNode result = mergeNode;
           while(l1 != null && l2 != null){
               if (l1.val > l2.val){
                   mergeNode.next = l2;
                   l2 = l2.next;
                   mergeNode = mergeNode.next;
               }else{
                   mergeNode.next = l1;
                   l1 = l1.next;
                   mergeNode = mergeNode.next;
               }
           }
           if(l1!=null){mergeNode.next = l1;}
           if(l2!=null){mergeNode.next = l2;}
           return result;
       }
   }
   ```

> 面试题26：树的子结构

1. [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree) (572)

   > Description

   Given two non-empty binary trees **s** and **t**, check whether tree **t** has exactly the same structure and node values with a subtree of **s**. A subtree of **s** is a tree consists of a node in **s** and all of this node's descendants. The tree **s** could also be considered as a subtree of itself.

   **Example 1:**
   Given tree s:

   ```
        3
       / \
      4   5
     / \
    1   2
   ```

   Given tree t:

   ```
      4 
     / \
    1   2
   ```

   Return true, because t has the same structure and node values with a subtree of s.

   **Example 2:**
   Given tree s:

   ```
        3
       / \
      4   5
     / \
    1   2
       /
      0
   ```

   Given tree t:

   ```
      4
     / \
    1   2
   ```

   Return false

   > Code_Python

   ```python
   class Solution:
       def is_same(self,s,t):
           if not s and not t:return True
           if not s or not t:return False
           if s.val != t.val:return False
           return self.is_same(s.left,t.left) and self.is_same(s.right,t.right)
       def isSubtree(self, s, t):
           """
           :type s: TreeNode
           :type t: TreeNode
           :rtype: bool
           """
           if not s:return False
           if self.is_same(s,t):return True
           return self.isSubtree(s.left,t) or self.isSubtree(s.right,t)
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
       public boolean isSubtree(TreeNode s, TreeNode t) {
           //如果树为空，则s为空，则返回false
           if(s == null){return false;}
           //如果子树匹配，则返回true
           if(isSame(s,t)){return true;}
           return isSubtree(s.left,t) || isSubtree(s.right,t);
       }
       //判断子树是否相同
       public boolean isSame(TreeNode s,TreeNode t){
           //如果遍历到最后为空，则返回true
           if(s==null && t==null){return true;}
           //如果一个为空另一个非空则返回false
           if(s==null || t==null){return false;}
           //如果两个值不同则返回false
           if(s.val != t.val){return false;}
           //依次遍历左子树喝右子树
           return (isSame(s.left,t.left) && isSame(s.right,t.right));
       }
   }
   ```
