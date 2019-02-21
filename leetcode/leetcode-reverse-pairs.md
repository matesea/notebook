# Problem

Given an array nums, we call (i, j) an important reverse pair if i < j and nums[i] > 2*nums[j].
You need to return the number of important reverse pairs in the given array.

**Exmaple 1:**
```
Input: [1,3,2,3,1]
Output: 2
```

**Example 2:**
```
Input: [2,4,3,5,1]
Output: 3
```

**Note:**
1. The length of the given array will not exceed 50,000.
2. All the numbers in the input array are in the range of 32-bit integer.

# Analysis
Please check the reference for detailed code
In brief, there are two approaches
- divide and conquer, which leads to extended merge sort
- stable logn approach to compute the result, such as BIT & BST

# Reference
- https://leetcode.com/problems/reverse-pairs/solution/
- https://leetcode.com/problems/reverse-pairs/discuss/97268/General-principles-behind-problems-similar-to-%22Reverse-Pairs%22
