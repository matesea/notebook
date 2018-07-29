# Problem
Given a string s, partition s such that every substring of the partition is a palindrome.  
Return the minimum cuts needed for a palindrome partitioning of s.  
**Example**
```
Input: "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

# Analysis
DP. And this solution is the most simple solution I can found.  
cut[i] = minimal cuts essential to split s[0:i-1], length=i, as palindrome substrings.  
In worse case, for string with length=i, we can cut in i-1 times into i single-character strings, which are palindromes.

Then for every palindrome substring, check if we can find smaller cuts.  
i-j-1 i-j    i    i+j  
.......|-----|-----|  
dp[i+j+1], for s[0:i+j] = 1+dp[i-j], if s[i-j:i+j] is palindrome string.  
And do the examination again for the case palindrome string is even length.

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

# Reference
http://www.cnblogs.com/grandyang/p/4271456.html  
https://leetcode.com/problems/palindrome-partitioning-ii/discuss/42198/My-solution-does-not-need-a-table-for-palindrome-is-it-right-It-uses-only-O(n)-space.
