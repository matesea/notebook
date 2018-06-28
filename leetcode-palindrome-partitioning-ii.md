# Palindrome Partitioning II
## Problem
Given a string s, partition s such that every substring of the partition is a palindrome.  
  
Return the minimum cuts needed for a palindrome partitioning of s.  
**Example:**
```
Input: "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

## First thoughts(TLE)
It should be computed with DP, of course.  
But I cannot think of a way to compute it in O(n<sup>2</sup>) time complexity & O(n) at the beginning.  
dp[i][j]: the minimal cut from s[i] to s[j]  
dp[i][j] = 0 if i==j  
0 if s[i:j] is a palindrome  
1 if length is 2 and not a palindrome  
min(dp[i][k], dp[k][j]) for i < k && k < j  
  
the time & space complexity is O(n<sup>3</sup>) and O(n<sup>2</sup>), no matter call it recursively or in pure nested loops.
(recursive approach has no benefit since all indexes have to be went through, it will not save any time but incur overhead from recursive calls)

## Solution
Assume n is the length of s.  

1. better DP  
P[n][n]: true if s[i:j] is palindrome string, otherwise false
dp[i]: minimal cut needed in s[i:n]  

P[i][j] is true if P[i+1][j-1] is true AND s[i] == s[j]  
So shorter string needs to be checked before longer ones.  
dp[i] = min(dp[i], 1+dp[j+1]) if s[i:j] is palindrome.  

time complexity: O(n<sup>2</sup>)  
space complexity: O(n)  
```C++
class Solution {
public:
    int minCut(string s) {
        int len = s.size();
        bool P[len][len];
        int dp[len + 1];
        for (int i = 0; i <= len; ++i) {
            dp[i] = len - i - 1;
        }
        for (int i = 0; i < len; ++i) {
            for (int j = 0; j < len; ++j) {
                P[i][j] = false;
            }
        }
        for (int i = len - 1; i >= 0; --i) {
            for (int j = i; j < len; ++j) {
                if (s[i] == s[j] && (j - i <= 1 || P[i + 1][j - 1])) {
                    P[i][j] = true;
                    dp[i] = min(dp[i], dp[j + 1] + 1);
                }
            }
        }
        return dp[0];
    }
};
```
2. another better DP  
let cut[n] = the minimum cut of first n characters in s  
At first cut[n] = n-1, with no palindrome in the first n characters.  
then cut[n] = min(cut[n], 1+cut[i]) if s[i:n-1] is palindrome
So only O(n) space & O(n<sup>2</sup>) time needed.  

```C++
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        vector<int> cut(n+1, 0);  // number of cuts for the first k characters
        for (int i = 0; i <= n; i++) cut[i] = i-1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; i-j >= 0 && i+j < n && s[i-j]==s[i+j] ; j++) // odd length palindrome
                cut[i+j+1] = min(cut[i+j+1],1+cut[i-j]);

            for (int j = 1; i-j+1 >= 0 && i+j < n && s[i-j+1] == s[i+j]; j++) // even length palindrome
                cut[i+j+1] = min(cut[i+j+1],1+cut[i-j+1]);
        }
        return cut[n];
    }
};
```
## Reference
http://www.cnblogs.com/grandyang/p/4271456.html  
https://leetcode.com/problems/palindrome-partitioning-ii/discuss/42198/My-solution-does-not-need-a-table-for-palindrome-is-it-right-It-uses-only-O(n)-space.
