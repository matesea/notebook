# Wildcard Matching
### problem
implement wildcard pattern matching with support for '?' and '*'.
```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") false
isMatch("aa","aa") true
isMatch("aaa","aa") false
isMatch("aa", "*") true
isMatch("aa", "a*") true
isMatch("ab", "?*") true
isMatch("aab", "c*a*b") false
```

### analysis
For each element in s  
If *s==*p or *p == ? which means this is a match, then goes to next element s++ p++.  
If p=='*', this is also a match, but one or many chars may be available, so let us save this *'s position and the matched s position.  
If not match, then we check if there is a * previously showed up,  
       if there is no *,  return false;  
       if there is an *,  we set current p to the next element of *, and set current s to the next saved s position.  

e.g.  

abed  

?b*d**  

- a=?, go on, b=b, go on,
- e=*, save * position star=3, save s position ss = 3, p++
- e!=d,  check if there was a *, yes, ss++, s=ss; p=star+1
- d=d, go on, meet the end.
- check the rest element in p, if all are *, true, else false;

__Note that in char array, the last is NOT NULL, to check the end, use  "*p"  or "*p=='\0'".__

### code
```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.length(), n = p.length();
        int i = 0, j = 0, asterisk = -1, match;
        while (i < m) {
            if (j < n && p[j] == '*') {
                match = i; 
                asterisk = j++;
            }
            else if (j < n && (s[i] == p[j] || p[j] == '?')) {
                i++; 
                j++;
            }
            else if (asterisk >= 0) {
                i = ++match;
                j = asterisk + 1;
            }
            else return false;
        }
        while (j < n && p[j] == '*') j++;
        return j == n;
    }
};
```
**Another approach**
DP
```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();
        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        dp[0][0] = true;
        for (int i = 1; i <= n; ++i) {
            if (p[i - 1] == '*') dp[0][i] = dp[0][i - 1];
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (p[j - 1] == '*') {
                    dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
                } else {
                    dp[i][j] = (s[i - 1] == p[j - 1] || p[j - 1] == '?') && dp[i - 1][j - 1];
                }
            }
        }
        return dp[m][n];
    }
};
```
### reference
[best solution](http://yucoding.blogspot.com/2013/02/leetcode-question-123-wildcard-matching.html)  
http://www.cnblogs.com/grandyang/p/4401196.html
