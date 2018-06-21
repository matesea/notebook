# Best time to Buy & Sell Stock with Bounded Transactions
### Problem

Refer leetcode

### Analysis

*k* is the upper bound of transactions
*i* is the day index
```
dp[i][k] = max(dp[i-1][k], prices[i] - prices[j] + dp[j-1][k-1]), for 0<= j < i
```

### Reference

[DP Solution](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/discuss/135704/Detail-explanation-of-DP-solution)
Check out for how to reduce its time complexity
