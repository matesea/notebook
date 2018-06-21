# Best time to Buy & Sell Stock with Cooldown
### Problem
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

- You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
- After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

__Example:__
```
Input: [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```
### Analysis
*sell[i]* is the profit so far that on day *i* I don't have hold stock.
*buy[i]* is the profit so far that on day *i* I have one hold stock.

sell[i] = max(sell[i-1], buy[i-1] + price)
buy[i] = max(buy[i-1], sell[i-2] + price)
