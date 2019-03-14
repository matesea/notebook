# Problem

In a given array nums of positive integers, find three non-overlapping subarrays with maximum sum.

Each subarray will be of size k, and we want to maximize the sum of all 3\*k entries.  

Return the result as a list of indices representing the starting position of each interval (0-indexed). If there are multiple answers, return the lexicographically smallest one.

**Example:**
```
Input: [1,2,1,2,6,7,5,1], 2
Output: [0, 3, 5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.
```

**Note:**
- nums.length will be between 1 and 20000.
- nums[i] will be between 1 and 65535.
- k will be between 1 and floor(nums.length / 3).

# Analysis
O(n) algorithm.  
Precompute sum of K elements then find the best choice at left/right hand sides.
Scan the entire array one by one to find an index, plus best in the right/left, to produce the solution
```C++
class Solution {
public:
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int K) {
        int sum = 0; int n = nums.size();
        vector<int> w;
        for (int i = 0; i < nums.size(); ++i) {
            sum += nums[i];
            if (i >= K) sum -= nums[i-K];
            if (i >= K-1) w.push_back(sum);
        }
        int ws = w.size();
        vector<int> left(ws, 0);
        vector<int> right(ws, n-K);
        int best = 0;
        for (int i = 0; i < ws; ++i) {
            if (w[i] > w[best]) best = i;
            left[i] = best;
        }
        best = n-K;
        for (int i = n-K; i >= 0; --i) {
            if (w[i] >= w[best]) best = i;
            right[i] = best;
        }
        vector<int> res(3, -1);
        for (int j = K; j+K < ws; ++j) {
            int i = left[j-K], k = right[j+K];
            if (res[0] == -1 || w[i]+w[j]+w[k] > w[res[0]]+w[res[1]]+w[res[2]]) {
                res[0] = i;
                res[1] = j;
                res[2] = k;
            }
        }
        return res;
    }
};
```

# Reference
- https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/solution/
- https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/discuss/108231/C%2B%2BJava-DP-with-explanation-O(n)
