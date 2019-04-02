# Problem
Let f(x) be the number of zeroes at the end of x!. (Recall that x! = 1 * 2 * 3 * ... * x, and by convention, 0! = 1.)

For example, f(3) = 0 because 3! = 6 has no zeroes at the end, while f(11) = 2 because 11! = 39916800 has 2 zeroes at the end. Given K, find how many non-negative integers x have the property that f(x) = K.

```
Example 1:
Input: K = 0
Output: 5
Explanation: 0!, 1!, 2!, 3!, and 4! end with K = 0 zeroes.

Example 2:
Input: K = 5
Output: 0
Explanation: There is no x such that x! ends in K = 5 zeroes.
```
**Note:**
- K will be an integer in the range [0, 10^9].

# Analysis
binary search
```C++
class Solution {
    long long numOfTrailingZeros(long long m) {
        long long res = 0;
        while (m > 0) {
            res += m/5;
            m /= 5;
        }
        return res;
    }
public:
    int preimageSizeFZF(int K) {
        for (long long l = 0, r = 5LL*(K+1); l <= r;) {
            long long m = (l+r)/2;
            long long k = numOfTrailingZeros(m);

            if (k < K)
                l = m+1;
            else if (k > K)
                r = m-1;
            else return 5;
        }
        return 0;
    }
};
```

# Reference
- https://leetcode.com/problems/preimage-size-of-factorial-zeroes-function/solution/
