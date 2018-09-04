# Problem
Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.
*Note:*
- 1 <= n <= 1000
- 1 <= m <= min(50, n)

*Examples:*
```
Input:
nums = [7,2,5,10,8]
m = 2

Output:
18

Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
```

# Analysis
DP approach  
dp[n, m] = min((max(dp[i, m-1], A[i]+...+A[n])))  
time complexity = O(n<sub>2</sub>m)
```C++
class Solution {
    inline int rangeSum(const vector<int>& sums, int i, int j) {
        // cout << i << "->" << j << ":" << sums[j+1] - sums[i] << endl;
        return sums[j+1] - sums[i];
    }
    int help(const vector<int>& sums, const vector<int>& maxs, int n, int m, vector<vector<int>>& dp) {
        int res = INT_MAX;
        if (dp[n][m]) return dp[n][m];
        if (m == 1) res = rangeSum(sums, 0, n-1);
        else if (n == m) res = maxs[n-1];
        else if (n > m) {
           for (int i = m-1; i < n; ++i)
               res = min(res, max(help(sums, maxs, i, m-1, dp), rangeSum(sums, i, n-1)));
        }
        return dp[n][m] = res;
    }
public:
    int splitArray(vector<int>& nums, int m) {
        vector<int> sums, maxs;
        vector<vector<int>> dp(nums.size()+1, vector<int>(m+1, 0));
        int sum = 0, mx = 0;
        sums.push_back(sum);
        for (const auto& n: nums) {
            mx = max(mx, n);
            sum += n;
            sums.push_back(sum);
            maxs.push_back(mx);
        }
        return help(sums, maxs, nums.size(), m, dp);
    }
};
```
Binary Search  
time complexity = O(nlgS), S: sum of nums
```C++
class Solution {
    bool valid(const vector<int>& nums, int m, int mid) {
        int count = 1, sum = 0;
        for (const auto& n: nums) {
            sum += n;
            if (sum > mid) {
                sum = n;
                count++;
                if (count > m) return false;
            }
        }
        return true;
    }
public:
    int splitArray(vector<int>& nums, int m) {
        int lo = 0, hi = 0;
        for (const auto& n: nums) {
            lo = max(lo, n);
            hi = hi + n;
        }
        if (m == 1) return hi;
        if (m == nums.size()) return lo;
        while (lo <= hi) {
            int mid = (lo+hi)/2;
            if (valid(nums, m, mid))
                hi = mid-1;
            else lo = mid+1;
        }
        return lo;
    }
};
```
# Reference
- https://leetcode.com/problems/split-array-largest-sum/discuss/89817/Clear-Explanation:-8ms-Binary-Search-Java
- https://leetcode.com/problems/split-array-largest-sum/discuss/89816/DP-Java
