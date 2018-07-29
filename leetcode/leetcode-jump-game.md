# Problem
Given an array of non-negative integers, you are initially positioned at the first index of the array.  
Each element in the array represents your maximum jump length at that position.  
Your goal is to reach the last index in the minimum number of jumps.  
**Example:**
```
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
Note:  
You can assume that you can always reach the last index.

# Analysis
Both jump game I & II have O(n) solution.  
- jump game: check array one by one and update the maximal distance we can get.  
- jump game II  
*level* is current steps  
*current* is the maximal distance in *level* steps from the beginning.  
*next* is the maximal reachable distance in next level.
```C++
class Solution {
    public:
    int jump(vector<int>& nums) {
        if (nums.size() < 2) return 0;
        int level = 0, current = 0, next = 0, i = 0;
        while (current-i+1 > 0) {
            level++;
            for (;i <= current; ++i) {
                next = max(next, i+nums[i]);
                if (next+1 >= nums.size()) return level;
            }
            current = next;
        }
        return 0;
    }
};
```

# Reference
http://www.cnblogs.com/grandyang/p/4373533.html
