### 剑指offer——LeetCode~39-52

##### 5 优化实践和空间效率

###### 5.2 时间效率

> 面试题39：数组中出现次数超过一半的数字

1. [Majority Element](https://leetcode.com/problems/majority-element) (169) 

   > Description

   Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

   You may assume that the array is non-empty and the majority element always exist in the array.

   **Example 1:**

   ```
   Input: [3,2,3]
   Output: 3
   ```

   **Example 2:**

   ```
   Input: [2,2,1,1,1,2,2]
   Output: 2
   ```

   > Code_Python

   ```python
   class Solution(object):
       def majorityElement(self, nums):
           """
           :type nums: List[int]
           :rtype: int
           中位数，因为我们要找的数字出现的次数比其它所有数字出现的次数之和还要多
           """
           result = -1
           times = 0
           for n in nums:
               if times==0:
                   result = n
                   times=1
               else: times += 1 if result == n else -1
           return result
   ```

   > Code_Java

   ```java
   class Solution {
       public int majorityElement(int[] nums) {
           //记录某个数字出现的次数，因为大于一般所以这个数字的总次数大于所有其它数字的次数
           int times = 1;
           //记录结果的数字
           int result = nums[0];
           for(int i = 1;i<nums.length;i++){
               if(times==0){
                   result=nums[i];
                   times = 1;
               }else{
                   if(result==nums[i]){
                       times++;
                   }else{
                       times--;
                   }
               }          
           }
           return result;
       }
   }
   ```

> 面试题40：最小的k个数

1. [LeetCode**无

   > Description

   对于一个无序数组，数组中元素为互不相同的整数，请返回其中最小的k个数，顺序与原数组中元素顺序一致。

   给定一个整数数组**A**及它的大小**n**，同时给定**k**，请返回其中最小的k个数。

   > Code_Python

   ```python
   # -*- coding:utf-8 -*-
   import heapq
   class KthNumbers:
       def findKthNumbers(self, A, n, k):
           # write code here
           if not A or k<=0 or k>len(A):return 
           max_heap = []
           for a in A:
               max_num = -a
               if len(max_heap)<k:
                   heapq.heappush(max_heap,max_num)
               else:
                   heapq.heappushpop(max_heap,max_num)
           return [a for a in A if (a in map(lambda x: -x, max_heap))]
   ```

   > Code_Java

   ```java
   
   ```

> 面试题41：数据流中的中位数

1. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream) (295) 

   > Description

   Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

   For example,

   `[2,3,4]`, the median is `3`

   `[2,3]`, the median is `(2 + 3) / 2 = 2.5`

   Design a data structure that supports the following two operations:

   - void addNum(int num) - Add a integer number from the data stream to the data structure.
   - double findMedian() - Return the median of all elements so far.

   **Example:**

   ```
   addNum(1)
   addNum(2)
   findMedian() -> 1.5
   addNum(3) 
   findMedian() -> 2
   ```

   > Code_Python

   ```python
   class MedianFinder:
   
       def __init__(self):
           """
           initialize your data structure here.
           """
           self.heaps = [],[]
           self.heap_low = []
           self.heap_big = []
   
       def addNum(self, num):
           """
           :type num: int
           :rtype: void
           """
           heap_low,heap_big = self.heaps
           # 每次都先进入大顶堆，然后从大顶堆中出去一个小的进小顶堆
           heapq.heappush(heap_low,-heapq.heappushpop(heap_big, -num))
           # 如果大顶堆长度小于小顶堆，则从小顶堆出去一个到大顶堆
           if len(heap_big) < len(heap_low):
               heapq.heappush(heap_big,-heapq.heappop(heap_low))
   
   
       def findMedian(self):
           """
           :rtype: float
           """
           heap_low,heap_big = self.heaps
           if len(heap_big) == len(heap_low): return (-heap_big[0] + self.heap_low[0]) / 2.0
           return -heap_big[0]
   
   
   # Your MedianFinder object will be instantiated and called as such:
   # obj = MedianFinder()
   # obj.addNum(num)
   # param_2 = obj.findMedian()
   ```

   > Code_Java

   ```java
   class MedianFinder {
   	//定义最大堆和最小堆
       PriorityQueue<Integer> min = new PriorityQueue();
       PriorityQueue<Integer> max = new PriorityQueue(1000, Collections.reverseOrder());
   
       /** initialize your data structure here. */
       public MedianFinder() {
           
       }
       
       public void addNum(int num) {
           max.offer(num);
           min.offer(max.poll());
           if (max.size() < min.size()){
               max.offer(min.poll());
           }
       }
       
       public double findMedian() {
           if (max.size() == min.size()) return (max.peek() + min.peek()) /  2.0;
           else return max.peek();
       }
   }
   
   /**
    * Your MedianFinder object will be instantiated and called as such:
    * MedianFinder obj = new MedianFinder();
    * obj.addNum(num);
    * double param_2 = obj.findMedian();
    */
   ```

> 面试题42：连续子数组的最大和

1. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray) (53)

   > Description

   Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

   **Example:**

   ```
   Input: [-2,1,-3,4,-1,2,1,-5,4],
   Output: 6
   Explanation: [4,-1,2,1] has the largest sum = 6.
   ```

   **Follow up:**

   If you have figured out the O(*n*) solution, try coding another solution using the divide and conquer approach, which is more subtle.

   > Code_Python

   ```python
   class Solution:
       def maxSubArray(self, nums):
           """
           :type nums: List[int]
           :rtype: int
           """
           if not nums:return 0
           max_num = -float("inf")
           temp_sum = 0 
           for n in nums:
               if temp_sum<=0:
                   temp_sum = n
               else:
                   temp_sum+=n
               if temp_sum>max_num:
                   max_num = temp_sum
               
           return max_num
   ```

   > Code_Java

   ```java
   class Solution {
       public int maxSubArray(int[] nums) {
           if(nums==null || nums.length==0){
               return 0;
           }
           //用于存放每次的小于0前操作的临时的和
           int tempSum = 0;
           //用于存放最大的和
           int maxSum = Integer.MIN_VALUE;
           for(int i=0;i<nums.length;i++){
               //如果临时的和小于等于零，代表加上前边的已经会影响到后边的总体和大小故，舍去前边的
               if (tempSum<=0){
                   tempSum = nums[i];
               }else{
                   tempSum+=nums[i];
               }
               if(maxSum<tempSum){
                   maxSum = tempSum;
               }
               
           }
           return maxSum;
       }
   }
   ```

> 面试题43：1~n整数中1出现的次数

1. [Number of Digit One](https://leetcode.com/problems/number-of-digit-one) (233)

   > Description

   Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

   **Example:**

   ```
   Input: 13
   Output: 6 
   Explanation: Digit 1 occurred in the following numbers: 1, 10, 11, 12, 13.
   ```

   > Code_Python

   ```python
   class Solution:
       def countDigitOne(self, n):
           """
           :type n: int
           :rtype: int
           """
           # 534 = （个位1出现次数）+（十位1出现次数）+（百位1出现次数）=（53*1+1）+（5*10+10）+（0*100+100）= 214
           if not n:return 0
           count = 0
           base =1
           round = n
           while round>0:
               weight = round%10
               round=int(round/10)
               count+=round*base
               if weight==1:
                   count+=(n%base)+1
               elif weight>1:
                   count+=base
               base*=10
           return count
   ```

   > Code_Java

   ```java
   https://blog.csdn.net/yi_afly/article/details/52012593
   class Solution {
       public int countDigitOne(int n) {
           if(n<1)
           return 0;
           int count = 0;
           int base = 1;
           int round = n;
           while(round>0){
               int weight = round%10;
               round/=10;
               count += round*base;
               if(weight==1)
                   count+=(n%base)+1;
               else if(weight>1)
                   count+=base;
               base*=10;
           }
           return count;
       }
   }
   ```

> 面试题44：数字序列中某一位的数字

1. [Nth Digit](https://leetcode.com/problems/nth-digit) (400)

   > Description

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
           base ,digit,temp= 9,1,1
           while n>base*digit:
               n -= base*digit
               digit+=1
               temp +=base
               base*=10
           # n-1保证了上一个数字
           return int(str(temp+(n-1)//digit)[(n-1)%digit])
   ```

   > Code_Java

   ```java
   class Solution {
       public int findNthDigit(int n) {
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

> 面试题45：把数组排成最小的数

1. [Largest Number](https://leetcode.com/problems/largest-number) (179)

   Given a list of non negative integers, arrange them such that they form the largest number.

   **Example 1:**

   ```
   Input: [10,2]
   Output: "210"
   ```

   **Example 2:**

   ```
   Input: [3,30,34,5,9]
   Output: "9534330"
   ```

   **Note:** The result may be very large, so you need to return a string instead of an integer.

   > Code_Python

   ```python
   import functools
   //替换python2中得cmp
   class Solution:
       def largestNumber(self, nums):
           """
           :type nums: List[int]
           :rtype: str
           """
           comp = lambda a,b:1 if a+b>b+a else -1
           num_to_str = list(map(str,nums))
           num_to_str.sort(key=functools.cmp_to_key(comp),reverse = True)
           return '0' if num_to_str[0] == '0' else ''.join(num_to_str)
   ```

   > Code_Java

   ```java
   class Solution {
       public String largestNumber(int[] nums) {
           String[] numsStrings = new String[nums.length];
           //将数组转换成string类型的数组，防止相加以后数组越界
           for(int i=0;i<nums.length;i++){
               numsStrings[i] = String.valueOf(nums[i]);
           }
           //使用自带的排序算法，并且复写比较方法
           Arrays.sort(numsStrings , new myCom());
           StringBuilder stringBuilder = new StringBuilder();
           if (numsStrings[0].charAt(0) == '0') return "0";
           for (String i : numsStrings) {
               stringBuilder.append(i);
           }
           return stringBuilder.toString();
           
       }
       class myCom implements Comparator<String> {
           @Override
           public int compare(String a , String b) {
               Long first = Long.parseLong(a.concat(b));
               Long second = Long.parseLong(b.concat(a)); // concat API is faster than the operator"+"
               if (first == second) return 0;
               return first < second ? 1 : -1;
           }
       }
   }
   ```

> 面试题46：把数字翻译成字符串

1. LeetCode**无

   给定一个数字，按照如下规则翻译成字符串：0翻译成“a”，1翻译成“b”…25翻译成“z”。一个数字有多种翻译可能，例如12258一共有5种，分别是bccfi，bwfi，bczi，mcfi，mzi。实现一个函数，用来计算一个数字有多少种不同的翻译方法。 

   > Code_Python

   ```python
   def numDecodings(s):
       if not s: return 0
       dp = [0]*(len(s)+1)
       dp[0] = 1
       dp[1] = 1 if s[0] != '0' else 0
       for i in range(2, len(s) + 1):
           first = s[i - 1]
           secode =  str(s[i - 2])+str(first)
           print(secode)
           if int(first) >= 1 and int(first) <= 9:
               dp[i] += dp[i - 1]
           if int(secode) >= 10 and int(secode) <= 26:
               dp[i] += dp[i - 2]
       return dp
   ```

   > Code_Java

   ```java
   public class Solution {
       public int numDecodings(String s) {
           if(s == null || s.length() == 0) {
               return 0;
           }
           int n = s.length();
           int[] dp = new int[n+1];
           dp[0] = 1;
           dp[1] = s.charAt(0) != '0' ? 1 : 0;
           for(int i = 2; i <= n; i++) {
               int first = Integer.valueOf(s.substring(i-1, i));
               int second = Integer.valueOf(s.substring(i-2, i));
               if(first >= 1 && first <= 9) {
                  dp[i] += dp[i-1];  
               }
               if(second >= 10 && second <= 26) {
                   dp[i] += dp[i-2];
               }
           }
           return dp[n];
       }
   }
   ```

   

> 面试题47：礼物的最大价值

1. [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum) (64)  

   Given a *m* x *n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.

   **Note:** You can only move either down or right at any point in time.

   **Example:**

   ```
   Input:
   [
     [1,3,1],
     [1,5,1],
     [4,2,1]
   ]
   Output: 7
   Explanation: Because the path 1→3→1→1→1 minimizes the sum.
   ```

   > Code_Python

   ```python
   class Solution:
       def minPathSum(self, grid):
           """
           :type grid: List[List[int]]
           :rtype: int
           """
           # 递推公式：dp[i,j] = grid[i][j]+min(dp[i-1][j],dp[i][j-1])
           m,n = len(grid),len(grid[0])
           dp = [[0 for i in range(n)] for j in range(m)]
           dp[0][0]= grid[0][0]
           # 给第一行赋值
           for i in range(1,len(grid[0])):dp[0][i] = dp[0][i-1]+grid[0][i]
           #给第一列赋值
           for j in range(1,len(grid)):dp[j][0] = dp[j-1][0]+grid[j][0]
           for col in range(1,len(grid[0])):
               for row in range(1,len(grid)):
                   dp[row][col] = grid[row][col] + min(dp[row-1][col],dp[row][col-1])
           return dp[-1][-1]
       
       
   #o(n)
   class Solution(object):
       def minPathSum(self, grid):
           """
           :type grid: List[List[int]]
           :rtype: int
           """
           dp = [0]*len(grid)
           dp[0] = grid[0][0]
           for i in range(1,len(grid)) :
               dp[i] = dp[i-1] + grid[i][0]
           for j in range(1, len(grid[0])) :
               for i in range(len(grid)) :
                   dp[i] = min(dp[i], dp[i-1]) + grid[i][j] if i > 0 else dp[i]+grid[i][j]
           return dp[len(grid)-1]
   ```

   > Code_Java

   ```java
   class Solution {
       public int minPathSum(int[][] grid) {
           //时间复杂度为o(n)，o(n^2)参看python
           int[] dp = new int[grid.length];
           dp[0] = grid[0][0];
           //给第一列赋值
           for(int i =1;i<grid.length;i++){
               dp[i] = dp[i-1]+grid[i][0];
           }
           for(int i=1 ; i<grid[0].length;i++){
               for(int j = 0;j<grid.length;j++){
                   dp[j] = (j==0 ? dp[j] : Math.min(dp[j], dp[j-1])) + grid[j][i];
               }
           }
           return dp[grid.length-1];
       }
   }
   ```

> 面试题48：最长不含重复字符的子字符串

1. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters) (3)

   > Description

   Given a string, find the length of the **longest substring** without repeating characters.

   **Examples:**

   Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.

   Given `"bbbbb"`, the answer is `"b"`, with the length of 1.

   Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a **substring**, `"pwke"` is a *subsequence* and not a substring.

   > Code_Python

   ```python
   class Solution(object):
       def lengthOfLongestSubstring(self,s):
           #存放下标的字典,存放结果的result以及指向重复开头的指针
           s_dict,result,j = {},0,0
           for i,ch in enumerate(s):
               if ch in s_dict.keys() and s_dict[ch]>=j:
                   j = s_dict[ch]+1
               s_dict[ch],result=i,max(result,i-j+1)
   	return result            
   ```

   > Code_Java

   ```java
   class Solution {
       public int lengthOfLongestSubstring(String s) {
           //ASCII 共有256个字符，，故创建一个256数组代替map，性能更好
           int[] charArrays = new int[256];
           int result = 0;
           int indexStart = 0;
           for(int i = 0 ;i<s.length();i++){
               if(charArrays[s.charAt(i)]==0 || charArrays[s.charAt(i)]<indexStart){
                   result = Math.max(result,i - indexStart + 1);
               }else{
                   indexStart = charArrays[s.charAt(i)];
                   
               }
               charArrays[s.charAt(i)] = i+1;
           }
           return result;
       }
   }
   ```

###### 5.3. 时间效率与空间效率的平衡

> 面试题49：丑数

1. [Ugly Number](https://leetcode.com/problems/ugly-number) (263)

   > Description

   Write a program to check whether a given number is an ugly number.

   Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`.

   **Example 1:**

   ```
   Input: 6
   Output: true
   Explanation: 6 = 2 × 3
   ```

   **Example 2:**

   ```
   Input: 8
   Output: true
   Explanation: 8 = 2 × 2 × 2
   ```

   **Example 3:**

   ```
   Input: 14
   Output: false 
   Explanation: 14 is not ugly since it includes another prime factor 7.
   ```

   **Note:**

   1. `1` is typically treated as an ugly number.
   2. Input is within the 32-bit signed integer range: [−231,  231 − 1].

   > Code_Python

   ```python
   class Solution:
       def isUgly(self, num):
           """
           :type num: int
           :rtype: bool
           """
           if not num:return False
           while num%2==0:num/=2
           while num%3==0:num/=3
           while num%5==0:num/=5
           return num==1
   ```

   > Code_Java

   ```java
   class Solution {
       public boolean isUgly(int num) {
           if (num <= 0) return false;
           while (num % 2 == 0) num /= 2;
           while (num % 3 == 0) num /= 3;
           while (num % 5 == 0) num /= 5;
           return num == 1;
       }
   }
   ```

2. [Ugly Number II](https://leetcode.com/problems/ugly-number-ii) (264)

   > Description

   Write a program to find the `n`-th ugly number.

   Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`. 

   **Example:**

   ```
   Input: n = 10
   Output: 12
   Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
   ```

   **Note:**  

   1. `1` is typically treated as an ugly number.
   2. `n` **does not exceed 1690**.

   > Code_Python

   ```python
   class Solution:
       def nthUglyNumber(self, n):
           """
           :type n: int
           :rtype: int
           """
           if not n:return 
           if n == 1:return 1
           result_list = []
           # 初始化，再此基础上相乘
           first,second,third = 2,3,5
           first_index,second_index,third_index =1,1,1
           result_list.append(1)
           # 当列表长度满足条件，结束
           while len(result_list)<n:
               min_num = min(first,second,third)
               result_list.append(min_num)
               if min_num == first:
                   first = result_list[first_index]*2
                   first_index = first_index+1
               if min_num == second:
                   second = result_list[second_index]*3
                   second_index =second_index+1
               if min_num == third:
                   third=result_list[third_index]*5
                   third_index = third_index+1
           return result_list[-1]
   ```

   > Code_Java

   ```java
   class Solution {
       public int nthUglyNumber(int n) {
           if(n <= 0) return 0;
           int[] res = new int[n];
           res[0] = 1;
           //维护u2，u3，u5这三个指针，指向丑数数组中的某个丑数
           int i2 = 0,i3 = 0,i5 = 0;
           //下一个丑数的位置
           int index = 1;
           while(index < n){
               //找出 U2* 2，U3* 3，U5* 5 中最小的那个
               int m2 = res[i2] * 2, m3 = res[i3] * 3, m5 = res[i5] * 5;
               int min = Math.min(Math.min(m2,m3),m5);
               //令下一个丑数等于这三个值中最小的那个
               res[index] = min;
               //去更新u2，u3，u5这三个指针
               if(m2==min){i2++;}
               if(m3==min){i3++;}
               if(m5==min){i5++;}
               index++;
           }
           return res[n-1];
       }
   }
   ```

> 面试题50：第一个只出现一次的字符

1. [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string) (387)

   > Description

   Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

   **Examples:**

   ```
   s = "leetcode"
   return 0.
   
   s = "loveleetcode",
   return 2.
   ```

   **Note:** You may assume the string contain only lowercase letters.

   > Code_Python

   ```python
   class Solution:
       def firstUniqChar(self, s):
           """
           :type s: str
           :rtype: int
           """
           # 一个字典，遍历两次
           if not s:return -1
           s_dict = {}
           for char in s:
               if char in s_dict:
                   s_dict[char] = s_dict[char]+1
               else:
                   s_dict[char] = 1
           for i,char in enumerate(s):
               if s_dict[char] == 1:return i
           return -1
   ```

   > Code_Java

   ```java
   class Solution {
       public int firstUniqChar(String s) {
           //数组代替map
           if(s == null || s.isEmpty()) {
               return -1;
           }
           int[] c = new int[26];
           char[] s1 = s.toCharArray();
           
           for(int i =0; i < s1.length; ++i) {
               c[s1[i] - 'a']++;
           }
           
           
           for(int i =0; i < s1.length; ++i) {
               if(c[s1[i] - 'a'] == 1) {
                   return i;
               }
           }
           return -1;
       }
   }
   ```

> 面试题51：数组中的逆序对

1. 

   > Description

   You are given an integer array *nums* and you have to return a new *counts* array. The *counts* array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

   **Example:**

   ```
   Input: [5,2,6,1]
   Output: [2,1,1,0] 
   Explanation:
   To the right of 5 there are 2 smaller elements (2 and 1).
   To the right of 2 there is only 1 smaller element (1).
   To the right of 6 there is 1 smaller element (1).
   To the right of 1 there is 0 smaller element.
   ```

   > Code_Python

   

> 面试题52：两个链表的第一个公共节点

1. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists) (160)

   > Description  

   Write a program to find the node at which the intersection of two singly linked lists begins.

    

   For example, the following two linked lists:

   ```
   A:          a1 → a2
                      ↘
                        c1 → c2 → c3
                      ↗            
   B:     b1 → b2 → b3
   ```

   begin to intersect at node c1.

    

   **Notes:**

   - If the two linked lists have no intersection at all, return `null`.
   - The linked lists must retain their original structure after the function returns.
   - You may assume there are no cycles anywhere in the entire linked structure.
   - Your code should preferably run in O(n) time and use only O(1) memory.

   **Credits:**
   Special thanks to [@stellari](https://oj.leetcode.com/discuss/user/stellari) for adding this problem and creating all test cases.

   > Code_Python

   ```python
   # Definition for singly-linked list.
   # class ListNode(object):
   #     def __init__(self, x):
   #         self.val = x
   #         self.next = None
   # 方法一
   class Solution(object):
       def getIntersectionNode(self, headA, headB):
           """
           :type head1, head1: ListNode
           :rtype: ListNode
           """
           patch_headA = headA
           patch_headB = headB
           if not headA or not headB:return None
           if headA.val == headB.val:return headA
           # caculate length of headA
           lengthA = 0
           while patch_headA:lengthA += 1;patch_headA=patch_headA.next
           # caculate length of headB
           lengthB = 0
           while patch_headB:lengthB += 1;patch_headB=patch_headB.next
           length = lengthA-lengthB
           print(length)
           if length<0:
               for _ in range(lengthB-lengthA):
                   headB=headB.next
           else:
               for _ in range(lengthA-lengthB):
                   headA=headA.next
           while headA:
               if headA.val==headB.val:
                   return headA
               headA=headA.next
               headB=headB.next
           return None
           
   #方法二
   class Solution(object):
       def getIntersectionNode(self, headA, headB):
           """
           :type head1, head1: ListNode
           :rtype: ListNode
           """
           
           a = set()
           while headA:
               a.add(headA)
               headA = headA.next
               
           while headB:
               if headB in a:
                   return headB
               headB = headB.next
           return None
   
   ```

   > Code_Java

   ```java
   public class Solution {
       public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
           ListNode pa = headA, pb = headB;
           while (pa != pb) {
               pa = (pa != null) ? pa.next : headB;
               pb = (pb != null) ? pb.next : headA;
           }
           return pa;
       }
   }
   ```

> 面试题53：在排序数组中查找数字

1. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array) (34)

   > Description

   Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

   Your algorithm's runtime complexity must be in the order of *O*(log *n*).

   If the target is not found in the array, return `[-1, -1]`.

   **Example 1:**

   ```
   Input: nums = [5,7,7,8,8,10], target = 8
   Output: [3,4]
   ```

   **Example 2:**

   ```
   Input: nums = [5,7,7,8,8,10], target = 6
   Output: [-1,-1]
   ```

   > Code_Python

   ```python
   class Solution(object):
       def searchRange(self, nums, target):
           """
           :type nums: List[int]
           :type target: int
           :rtype: List[int]
           """
           # 利用二分查找左边，二分查找，找到中间节点等于目标节点即停止，这里找到以后会继续在左边数组中查找
           def search_first(nums,target):
               idx = -1
               start = 0
               end = len(nums)-1
               while start<=end:
                   mid = (start+end)//2
                   if nums[mid]>=target:
                       end = mid-1
                   else:
                       start = mid+1
                   if nums[mid]==target:idx=mid
               return idx
           # 利用二分查找最后节点
           def search_last(nums,target):
               idx = -1
               start = 0
               end = len(nums)-1
               while start<=end:
                   mid = (start+end)//2
                   if nums[mid]<=target:
                       start = mid+1
                   else:
                       end = mid-1
                   if nums[mid]==target:idx=mid
               return idx
           first = search_first(nums,target)
           second = search_last(nums,target)
           return [first,second]
   ```

> 面试题54：二叉搜索树的第K大节点

1. [ Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst) (230)

   > Description

   Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

   **Note:** 
   You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

   **Example 1:**

   ```
   Input: root = [3,1,4,null,2], k = 1
      3
     / \
    1   4
     \
      2
   Output: 1
   ```

   **Example 2:**

   ```
   Input: root = [5,3,6,2,4,null,null,1], k = 3
          5
         / \
        3   6
       / \
      2   4
     /
    1
   Output: 3
   ```

   **Follow up:**
   What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

   > Code_Python

   ```python
   # 方法一
   # Definition for a binary tree node.
   # class TreeNode(object):
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Solution(object):
       def kthSmallest(self, root, k):
           """
           :type root: TreeNode
           :type k: int
           :rtype: int
           """
           global res, count
           res,count = 0,0
           def dfs(root, k) :
               if not root : return
               dfs(root.left, k)
               global count, res
               count += 1
               if count == k: res = root.val  
               dfs(root.right, k)
           dfs(root,k)
           return res
   # 方法二
   class Solution:
       def kthSmallest(self, root, k):
           """
           :type root: TreeNode
           :type k: int
           :rtype: int
           """
           i = 0
           stack = []
           while root or stack:
               if root:
                   stack.append(root)
                   root = root.left
               else:
                   node = stack.pop()
                   i += 1
                   if i == k:
                        return node.val
                   root = node.right  
   ```

> 面试题55：二叉树的深度

1. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree) (104)

   > Description

   Given a binary tree, find its maximum depth.

   The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

   **Note:** A leaf is a node with no children.

   **Example:**

   Given binary tree `[3,9,20,null,null,15,7]`,

   ```
       3
      / \
     9  20
       /  \
      15   7
   ```

   return its depth = 3.

   > Code_Python

   ```python
   # Definition for a binary tree node.
   # class TreeNode:
   #     def __init__(self, x):
   #         self.val = x
   #         self.left = None
   #         self.right = None
   
   class Solution:
       def maxDepth(self, root):
           """
           :type root: TreeNode
           :rtype: int
           """
           if not root:
               return 0
           left = self.maxDepth(root.left)
           right = self.maxDepth(root.right)
           return left+1 if left>right else right+1
   ```

   > Code_Java

   ```java
   /**
    * Definition for binary tree
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode(int x) { val = x; }
    * }
    */
   public class Solution {
       public int maxDepth(TreeNode root) {
           if(root == null)
               return 0;
          else if(root.left == null && root.right == null)
               return 1;
          else
               {
          			int leftDepth= maxDepth(root.left);
          			int rightDepth = maxDepth(root.right);
                  	 if(leftDepth > rightDepth)
                    	   return leftDepth +1;
                  	 else
                   	    return rightDepth +1;
                }
          
       }
   }
   ```

   

> 面试题56：

