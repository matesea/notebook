# Problem
Given an unsorted array nums, reorder it such that nums[0] < nums[1] > nums[2] < nums[3]....  
Example 1:
```
Input: nums = [1, 5, 1, 1, 6, 4]
Output: One possible answer is [1, 4, 1, 5, 1, 6].
```
Example 2:
```
Input: nums = [1, 3, 2, 2, 3, 1]
Output: One possible answer is [2, 3, 1, 3, 1, 2].
```
**Note:**
You may assume all input has valid answer.

**Follow Up:**
Can you do it in O(n) time and/or in-place with O(1) extra space?

# Analysis
1. use nth_element to find medium and partition nums into two parts.
2. use A(i) macro to transform index.  
    i     0 1 2 3 4 5 6  
    A(i)  1 3 5 0 2 4 6  
    odd indexes before even ones
3. do the partition above the macro to ensure that  
    a) A(:i-1): larger then medium   
    b) A(i:j-1): equal to medium  
    c) A(j:k): unprocessed elements  
    d) A(k+1:): less than medium
```C++
// O(1) space
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        #define A(i) nums[(1 + 2 * i) % (n | 1)]
        int n = nums.size(), i = 0, j = 0, k = n - 1;
        auto midptr = nums.begin() + n / 2;
        nth_element(nums.begin(), midptr, nums.end());
        int mid = *midptr;
        while (j <= k) {
            if (A(j) > mid) swap(A(i++), A(j++));
            else if (A(j) < mid) swap(A(j), A(k--));
            else ++j;
        }
    }
};
```

# Reference
http://www.cnblogs.com/grandyang/p/5139057.html
