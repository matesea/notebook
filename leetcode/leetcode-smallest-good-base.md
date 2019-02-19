# Problem
For an integer n, we call k>=2 a good base of n, if all digits of n base k are 1.

Now given a string representing n, you should return the smallest good base of n in string format.

**Example 1:**
```
Input: "13"
Output: "3"
Explanation: 13 base 3 is 111.
```

**Example 2:**
```
Input: "4681"
Output: "8"
Explanation: 4681 base 8 is 11111.
```

**Example 3:**
```
Input: "1000000000000000000"
Output: "999999999999999999"
Explanation: 1000000000000000000 base 999999999999999999 is 11.
```
**Note:**  
1.The range of n is [3, 10^18].  
2.The string representing n is always valid and will not have leading zeros.

# Analysis
1+k+k^2+k^3...+k^m = n
by sum of geometric series, (k^(m+1)-1)/(k-1) = n
since k >= 2, 2^(m+1)-1 = n, m = log2(n+1)-1 when k = 2, so m <= log2(n+1)-1
smallest k means maximal m, so we look for m from its maximal value down to 2

besides, n^(1/(m+1)) < k < n^(1/m)
here we can take advantage of binary search to locate if there exists k such that 1+k+k^2...+k^m = n

```C++
class Solution {
public:
    string smallestGoodBase(string num) {
        unsigned long long n = stoull(num);
        for (int i = log2(n+1)-1; i >= 2; --i) {
            int lo = pow(n, 1.0/(i+1));
            int hi = pow(n, 1.0/i)+1;
            while (lo <= hi) {
                int mid = (lo+hi)/2;
                unsigned long long cur = mid;
                unsigned long long sum = 1;
                for (int j = 1; j <= i; ++j) {
                    sum += cur;
                    cur *= mid;
                }
                if (sum == n)
                    return to_string(mid);
                if (sum < n)
                    lo = mid+1;
                else hi = mid-1;
            }
        }
        return to_string(n-1);
    }
};
```

# Reference
- https://leetcode.com/problems/smallest-good-base/discuss/96587/Python-solution-with-detailed-mathematical-explanation-and-derivation
- https://leetcode.com/problems/smallest-good-base/discuss/96590/3ms-AC-C++-long-long-int-+-binary-search
- http://www.cnblogs.com/grandyang/p/6620351.html
