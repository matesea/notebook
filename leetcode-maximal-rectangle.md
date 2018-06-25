# maximal rectangle

## problem
Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.
```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

## Analysis
1. use *height* array to calculate current height row by row.
then use the algorithm in *Largest Rectangle in Histogram* to compute the maximal area so far.
2. use two arrays, *left* and *right*, to record the 1st & last index of continuous 1's sequence.
*height* to keep current height as well.
then compute area = height[j] * (left[j] - right[j]) then update area

## Reference
http://www.cnblogs.com/grandyang/p/4322667.html