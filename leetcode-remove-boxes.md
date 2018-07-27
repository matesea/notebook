# problem
Given several boxes with different colors represented by different positive numbers. 
You may experience several rounds to remove boxes until there is no box left. Each time you can choose some continuous boxes with the same color (composed of k boxes, k >= 1), remove them and get k\*k points.
Find the maximum points you can get.

Example 1:
Input:
```
[1, 3, 2, 2, 2, 3, 4, 3, 1]
```
Output:
```
23
```
Explanation:
```
[1, 3, 2, 2, 2, 3, 4, 3, 1] 
----> [1, 3, 3, 4, 3, 1] (3*3=9 points) 
----> [1, 3, 3, 3, 1] (1*1=1 points) 
----> [1, 1] (3*3=9 points) 
----> [] (2*2=4 points)
```

# analysis
check the reference for why optimal substructure property is not obeyed with T(i,j) two-dimension DP approach.
```C++
class Solution {
    int removeBoxes(vector<int>& boxes, int i, int j, int k, vector<vector<vector<int>>>& dp) {
        if (i > j) return 0;
        if (dp[i][j][k] > 0) return dp[i][j][k];
        for (; i+1 <= j && boxes[i] == boxes[i+1]; ++i, ++k);
        int res = (k+1)*(k+1) + removeBoxes(boxes, i+1, j, 0, dp);
        for (int m = i+1; m <= j; ++m)
            if (boxes[m] == boxes[i])
                res = max(res, removeBoxes(boxes, i+1, m-1, 0, dp) + removeBoxes(boxes, m, j, k+1, dp));
        return dp[i][j][k] = res;
    }
public:
    int removeBoxes(vector<int>& boxes) {
        int n = boxes.size();
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(n, 0)));
        return removeBoxes(boxes, 0, n-1, 0, dp);
    }
};
```


# reference
https://leetcode.com/problems/remove-boxes/discuss/101310/Java-top-down-and-bottom-up-DP-solutions
