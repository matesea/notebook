# Problem

Given an unsorted array of integers, find the number of longest increasing subsequence.

**Example 1:**
```
Input: [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].
```

**Example 2:**
```
Input: [2,2,2,2,2]
Output: 5
Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
```

**Note:** Length of the given array will be not exceed 2000 and the answer is guaranteed to be fit in 32-bit signed int.

# Analysis
- dynamic programming in O(n<sup>2</sup>)
```C++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        if (!nums.size()) return  0;
        int n = nums.size();
        vector<int> len(nums.size(), 1);
        vector<int> count(nums.size(), 1);
        int maxlen = 1, res = 0;;
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < j; ++i) {
                if (nums[i] >= nums[j]) continue;
                if (len[i] >= len[j]) {
                    len[j] = len[i] + 1;
                    count[j] = count[i];
                    maxlen = max(maxlen, len[j]);
                } else if (len[i] + 1== len[j])
                    count[j] += count[i];
            }
        }
        for (int i = 0; i < n; ++i)
            if (len[i] == maxlen)
                res += count[i];
        return res;
    }
};
```

- segment tree in O(nlgn)
the idea is same as 315 count of smaller numbers after self
```C++
class Solution {
    pair<int,int> query(vector<pair<int,int>>& st, int i) {
        int n = st.size()/2;
        int lo = n, hi = n+i;
        int len = 0, count = 1;
        if (i < 0) return {0, 1};
        while (lo <= hi) {
            if (lo&1) {
                if (st[lo].first > len) {
                    len = st[lo].first;
                    count = st[lo].second;
                } else if (st[lo].first == len)
                    count += st[lo].second;
                lo++;
            }
            if (hi%2 == 0) {
                if (st[hi].first > len) {
                    len = st[hi].first;
                    count = st[hi].second;
                } else if (st[hi].first == len)
                    count += st[hi].second;
                --hi;
            }
            lo = lo/2;
            hi = hi/2;
        }
        return {len, count};
    }
    void update(vector<pair<int,int>>& st, int i, int len, int count) {
        int n = st.size()/2;
        i += n;
        if (st[i].first > len) return;
        while (i > 0) {
            if (st[i].first == len) st[i].second += count;
            else if (st[i].first < len) {
                st[i].first = len;
                st[i].second = count;
            }
            i = i/2;
        }
    }
public:
    int findNumberOfLIS(vector<int>& nums) {
        int n = nums.size();
        if (!n) return 0;
        vector<pair<int,int>> st(2*n, {0, 1});
        unordered_map<int,int> loc;
        vector<int> sorted(nums);
        sort(sorted.begin(), sorted.end());
        int i = 0;
        for (auto& s: sorted) {
            if (loc.find(s) == loc.end())
                loc[s] = i;
            ++i;
        }
        
        for (i = n-1; i > 0; --i)
            st[i].second = st[2*i].second + st[2*i+1].second;
        for (auto x: nums) {
            auto p = query(st, loc[x]-1);
            int len = p.first, count = p.second;
            update(st, loc[x], len+1, (len==0?1:count));
        }
        return st[1].second;
    }
};
```
# Reference
- https://leetcode.com/problems/number-of-longest-increasing-subsequence/discuss/107293/JavaC%2B%2B-Simple-dp-solution-with-explanation
- https://leetcode.com/problems/number-of-longest-increasing-subsequence/solution/
