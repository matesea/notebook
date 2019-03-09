# Problem

Given an array consisting of n integers, find the contiguous subarray whose length is greater than or equal to k that has the maximum average value. And you need to output the maximum average value.

**Example 1:**
```
Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
Explanation:
when length is 5, maximum average value is 10.8,
when length is 6, maximum average value is 9.16667.
Thus return 12.75.
```

**Note:**
1. 1 <= k <= n <= 10,000.
1. Elements of the given array will be in range [-10,000, 10,000].
1. The answer with the calculation error less than 10-5 will be accepted.

# Analysis
Binary search
```C++
class Solution {
    bool check(vector<int>& n, int k, double guess) {
        double sum = 0, sumFromBegin = 0, minSum = 0;
        for (int i = 0; i < k; ++i)
            sum += n[i]-guess;
        if (sum >= 0) return true;
        for (int i = k; i < n.size(); ++i) {
            sum += n[i]-guess;
            sumFromBegin += n[i-k]-guess;
            minSum = min(minSum, sumFromBegin);
            if (sum >= minSum) return true;
        }
        return false;
    }
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double left = *min_element(nums.begin(), nums.end());
        double right = *max_element(nums.begin(), nums.end());
        while (right-left >= 1e-6) {
            double mid = (left+right)*0.5;
            // cout << "left=" << left << ", right=" << right << ", mid=" << mid << endl;
            if (check(nums, k, mid))
                left = mid;
            else right = mid;
        }
        return left;
    }
};
```

# Reference
- https://leetcode.com/problems/maximum-average-subarray-ii/solution/
