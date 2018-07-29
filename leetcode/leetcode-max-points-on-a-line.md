# Max Points on a line
## Problem
Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.

**Example 1:**
```
Input: [[1,1],[2,2],[3,3]]
Output: 3
Explanation:
^
|
|        o
|     o
|  o  
+------------->
0  1  2  3  4
```
**Example 2:**
```
Input: [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
Output: 4
Explanation:
^
|
|  o
|     o        o
|        o
|  o        o
+------------------->
0  1  2  3  4  5  6
```

## Analysis
For each point A, compute the slope of point A & B.  
B is the point after A in the array.  
Then check if any duplicate slopes with hash.  

**Note:** same point counts  
time complexity: O(n<sup>2</sup>)

## Lessons
three ways to determine if three points laying on a line.  
1) slope  
(y<sub>3</sub> - y<sub>1</sub>) / (x<sub>3</sub> - x<sub>1</sub>) == (y<sub>2</sub> - y<sub>1</sub>) / (x<sub>2</sub> - x<sub>1</sub>)  

problem:  
a) pay attentions for the cases that dividers are 0  
b) using float as unit might be not accurate  

2) perimeter
sort distance: distance<sub>1,2</sub> > distance<sub>1,3</sub> > distance<sub>1,3</sub>  
distance<sub>1,2</sub> == distance<sub>1,3</sub> + distance<sub>1,3</sub>  

problem:  
sqrt to calculate distance might not accurate because it's floating point  

3) area = 0  
area = 1/2 * (Vector<sub>1,2</sub> x Vector<sub>1,3</sub>) = 0  
(x<sub>1</sub> - x<sub>3</sub>)*(y<sub>2</sub> - y<sub>3</sub>) == (x<sub>2</sub> - x<sub>3</sub>) * (y<sub>1</sub> - y<sub>3</sub>)  

## Reference
http://www.cnblogs.com/grandyang/p/4579693.html  
https://www.geeksforgeeks.org/count-maximum-points-on-same-line/  
http://yiminghe.iteye.com/blog/568666
