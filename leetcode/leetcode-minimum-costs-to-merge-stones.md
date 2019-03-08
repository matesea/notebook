# Problem
There are N piles of stones arranged in a row.  The i-th pile has stones[i] stones.

A move consists of merging exactly K consecutive piles into one pile, and the cost of this move is equal to the total number of stones in these K piles.

Find the minimum cost to merge all piles of stones into one pile.  If it is impossible, return -1.

**Example 1:**
```
Input: stones = [3,2,4,1], K = 2
Output: 20
Explanation: 
We start with [3, 2, 4, 1].
We merge [3, 2] for a cost of 5, and we are left with [5, 4, 1].
We merge [4, 1] for a cost of 5, and we are left with [5, 5].
We merge [5, 5] for a cost of 10, and we are left with [10].
The total cost was 20, and this is the minimum possible.
```

**Example 2:**
```
Input: stones = [3,2,4,1], K = 3
Output: -1
Explanation: After any merge operation, there are 2 piles left, and we can't merge anymore.  So the task is impossible.
```

**Example 3:**
```
Input: stones = [3,5,1,2,6], K = 3
Output: 25
Explanation: 
We start with [3, 5, 1, 2, 6].
We merge [5, 1, 2] for a cost of 8, and we are left with [3, 8, 6].
We merge [3, 8, 6] for a cost of 17, and we are left with [17].
The total cost was 25, and this is the minimum possible.
```

**Note:**
- 1 <= stones.length <= 30
- 2 <= K <= 30
- 1 <= stones[i] <= 100

# Analysis
This issue is very interesting.
Please follow reference links for the detailed explanations.
Here only the most important idea is mentioned.

- 3d DP
dp[i][j][m] means the cost needed to merge stone[i] ~ stones[j] into m piles.
(exactly m piles, which is very different from the 2d dp solution)

Initial status dp[i][i][1] = 0 and dp[i][i][m] = infinity

dp[i][j][1] = dp[i][j][k] + stonesNumber[i][j]
dp[i][j][m] = min(dp[i][mid][1] + dp[mid + 1][j][m - 1]) for i <= mid < j

- 2d DP
stones[i] ~ stones[j] will merge as much as possible.

Finally (j - i) % (K - 1) + 1 piles will be left.

It's less than K piles and no more merge happens.

dp[i][j] means the minimum cost needed to merge stones[i] ~ stones[j].

- although time complexity is same but much better for space saving

# Reference
- https://leetcode.com/problems/minimum-cost-to-merge-stones/discuss/247567/JavaC%2B%2BPython-DP
- https://leetcode.com/problems/minimum-cost-to-merge-stones/discuss/247657/JAVA-Bottom-Up-%2B-Top-Down-DP-With-Explaination
