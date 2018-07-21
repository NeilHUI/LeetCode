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

