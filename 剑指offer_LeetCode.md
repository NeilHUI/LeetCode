### 剑指offer——LeetCode

##### 2.3 数据结构

###### 2.3.1 数组

> 面试题3 数组中重复的数字

 1. Contains Duplicate (217)

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

 2. Contains Duplicate II (219)

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

    

 3. Contains Duplicate III (220)

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

1. Search a 2D Matrix (74)

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

2. Search a 2D Matrix II (240)

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

3. Contains Duplicate III (220)

   > 

   > 

   ```python
   
   ```

   > Code_Java

   ```java
   
   ```

> 面试题5 二维数组中的查找