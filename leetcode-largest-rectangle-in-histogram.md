# Problem
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

**Example:**  
Input: [2,1,5,6,2,3]  
Output: 10

# Code
```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int area = 0;
        stack<int> s;
        heights.push_back(0);
        for (int i = 0; i < heights.size(); ++i) {
            while (!s.empty() && heights[i] < heights[s.top()]) {
                int cur = s.top();
                s.pop();
                area = max(area, heights[cur] * (s.empty()?i:i-s.top()-1));
            }
            s.push(i);
        }
        return area;
    }
};
```

# Reference
http://www.cnblogs.com/grandyang/p/4322653.html  
https://www.geeksforgeeks.org/largest-rectangular-area-in-a-histogram-set-1/  
https://www.geeksforgeeks.org/largest-rectangle-under-histogram/
