# Problem
Find the length of the longest substring T of a given string (consists of lowercase letters only) such that every character in T appears no less than k times.  
Example 1:  
```
Input:
s = "aaabb", k = 3

Output:
3

The longest substring is "aaa", as 'a' is repeated 3 times.
```
Example 2:  
```
Input:
s = "ababbc", k = 2

Output:
5

The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

# Analysis
1. recursive approach
Count the frequency of each character.  
Then traverse s, for each character with frequency less than k, which breaks the target substring, check it recursively.  
time complexity: O(nlgn)
```C++
class Solution {
public:
    int longestSubstring(string s, int k) {
        int n = s.size(), max_idx = 0, res = 0;
        int m[128] = {0};
        bool ok = true;
        for (char c : s) ++m[c];
        for (int i = 0; i < n; ++i) {
            if (m[s[i]] < k) {
                res = max(res, longestSubstring(s.substr(max_idx, i - max_idx), k));
                ok = false;
                max_idx = i + 1;
            }
        }
        return ok ? n : max(res, longestSubstring(s.substr(max_idx, n - max_idx), k));
    }
};
```
2. iterative approach
find the longest substring starting from i to j, ranged from 0 ~ n.  
if it fits the criteria and its length > current maximum, update it.  
time complexity: O(n<sup>2</sup>)
```C++
class Solution {
public:
    int longestSubstring(string s, int k) {
        int res = 0, i = 0, n = s.size();
        while (i + k <= n) {
            int m[26] = {0}, mask = 0, max_idx = i;
            for (int j = i; j < n; ++j) {
                int t = s[j] - 'a';
                ++m[t];
                if (m[t] < k) mask |= (1 << t);
                else mask &= (~(1 << t));
                if (mask == 0) {
                    res = max(res, j - i + 1);
                    max_idx = j;
                }
            }
            i = max_idx + 1;
        }
        return res;
    }
};
```
3. divide and conquer
similar to 1, for the character with frequency < k and not be part of the target substring,  
divide s into two substring and check recursively.  
time complexity: O(nlgn)
```C++
class Solution {
public:
    int longestSubstring(string s, int k) {
        int n = s.size();
        return helper(s, 0, n-1, k);
    }
private:
    // looking for longest string within index range [l, r]
    int helper(string& s, int l, int r, int k) {
        vector<int> mp(26, 0);
        for (int i = l; i <= r; i++) mp[s[i]-'a']++;
       // check whether the whole string meets requirement
        bool pass = true;
        for (int i = 0; i < 26 && pass; i++) {
            if (mp[i] && mp[i] < k)
                pass = false;
        }
        if (pass) return r-l+1;
        // using all characters with occurrence > 0 && < k to divide the string
        int i = l, ans = 0;
        for (int j = l; j <= r; j++) {
            if (mp[s[j]-'a'] && mp[s[j]-'a'] < k) {
                ans = max(ans, helper(s, i, j-1, k));
                i = j+1;
            }
        }
        return max(ans, helper(s, i, r, k));
    }
};
```
# Reference
https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/discuss/87739/Java-Strict-O(N)-Two-Pointer-Solution  
https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/discuss/87741/Java-divide-and-conquer(recursion)-solution  
http://www.cnblogs.com/grandyang/p/5852352.html
