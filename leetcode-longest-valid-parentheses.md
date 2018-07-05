# Problem
Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.
**Example 1:**
```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```
**Example 2:**
```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

# Code
1. Scan from left to right and right to left and record the longest valid parentheses we found.
```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        int left = 0, right = 0;
        int maxlen = 0;
        for (unsigned int i = 0; i < s.length(); ++i) {
            if (s[i] == '(') left++;
            if (s[i] == ')') right++;
            if (left == right) maxlen = (maxlen > right * 2)? maxlen : right * 2;
            if (left < right) {left = 0; right = 0;}
        }
        
        right = 0;
        left = 0;
        for (int i = s.length() - 1; i >= 0; --i) {
            if (s[i] == ')') right++;
            if (s[i] == '(') left++;
            if (left == right) maxlen = (maxlen > left * 2) ? maxlen : left * 2;
            if (left > right) {left = 0; right = 0;}
        }
        return maxlen;
    }
};

```

2. Use stack to push/pop characters.  
calculate matched length every time a matched pair found.
```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        int res = 0, start = 0;
        stack<int> m;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '(') m.push(i);
            else if (s[i] == ')') {
                if (m.empty()) start = i + 1;
                else {
                    m.pop();
                    res = m.empty() ? max(res, i - start + 1) : max(res, i - m.top());
                }
            }
        }
        return res;
    }
};
```

3. DP  
dp[i] is the length of longest valid parentheses substring, ending at s[i-1].  
dp[0] = 0.  
So for s[i-1], we need longest valid parentheses ended at s[i-2], which is dp[i-1],  
and if s[i-1] == ')' and form a new pair with the one before dp[i-1].  
j = (i - 1) - dp[i-1] - 1  
if yes, dp[i] = dp[i-1]+2+dp[j]  

```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.size(), maxLen = 0;
        vector<int> dp(n+1,0);
        for(int i=1; i<=n; i++) {
            int j = i-2-dp[i-1];
            if(s[i-1]=='(' || j<0 || s[j]==')') 
                dp[i] = 0;
            else {
                dp[i] = dp[i-1]+2+dp[j];
                maxLen = max(maxLen, dp[i]);
            }
        }
        return maxLen;
    }
};
```

# Reference
http://www.cnblogs.com/grandyang/p/4424731.html  
http://bangbingsyb.blogspot.com/2014/11/leetcode-longest-valid-parentheses.html
