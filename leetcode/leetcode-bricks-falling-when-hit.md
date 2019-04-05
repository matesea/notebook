# Problem
We have a grid of 1s and 0s; the 1s in a cell represent bricks.  A brick will not drop if and only if it is directly connected to the top of the grid, or at least one of its (4-way) adjacent bricks will not drop.

We will do some erasures sequentially. Each time we want to do the erasure at the location (i, j), the brick (if it exists) on that location will disappear, and then some other bricks may drop because of that erasure.

Return an array representing the number of bricks that will drop after each erasure in sequence.

```
Example 1:
Input: 
grid = [[1,0,0,0],[1,1,1,0]]
hits = [[1,0]]
Output: [2]
Explanation: 
If we erase the brick at (1, 0), the brick at (1, 1) and (1, 2) will drop. So we should return 2.
```
```
Example 2:
Input: 
grid = [[1,0,0,0],[1,1,0,0]]
hits = [[1,1],[1,0]]
Output: [0,0]
Explanation: 
When we erase the brick at (1, 0), the brick at (1, 1) has already disappeared due to the last move. So each erasure will cause no bricks dropping.  Note that the erased brick (1, 0) will not be counted as a dropped brick.
```
**Note:**
- The number of rows and columns in the grid will be in the range [1, 200].
- The number of erasures will not exceed the area of the grid.
- It is guaranteed that each erasure will be different from any other erasure, and located inside the grid.
- An erasure may refer to a location with no brick - if it does, no bricks drop.

# Analysis
reverse union find
```C++
class Solution {
    class DSU {
        vector<int> sz;
        vector<int> rank;
        vector<int> root;
        public:
        DSU(int n) {
            sz.resize(n, 1);
            rank.resize(n, 0);
            root.resize(n, 0);
            for (int i = 0; i < n; ++i)
                root[i] = i;
        }
        int find(int p) {
            if (p != root[p])
                root[p] = find(root[p]);
            return root[p];
        }
        void join(int p, int q) {
            int rp = find(p), rq = find(q);
            if (rp == rq) return;
            if (rank[rp] < rank[rq])
                swap(rp, rq);
            if (rank[rp] == rank[rq])
                rank[rp]++;
            root[rq] = rp;
            sz[rp] += sz[rq];
        }
        int size(int p) {
            return sz[find(p)];
        }
        int top() {
            return size(sz.size()-1)-1;
        }
    };
public:
    vector<int> hitBricks(vector<vector<int>>& grid, vector<vector<int>>& hits) {
        int m = grid.size(), n = grid[0].size(), t = hits.size();
        DSU dsu(m*n+1);
        vector<int> res(t, 0);
        const vector<vector<int>> dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

        for (const auto& hit: hits) {
            const int& r = hit[0], c = hit[1];
            if (grid[r][c] == 0)
                grid[r][c] = -1;
            else if (grid[r][c] == 1)
                grid[r][c] = 0;
        }
        for (int r = 0; r < m; ++r) {
            for (int c = 0; c < n; ++c) {
                if (grid[r][c] != 1) continue;
                if (r == 0) dsu.join(r*n+c, m*n);
                if (r > 0 && grid[r-1][c] == 1)
                    dsu.join(r*n+c, (r-1)*n+c);
                if (c > 0 && grid[r][c-1] == 1)
                    dsu.join(r*n+c, r*n+c-1);
            }
        }
        while (--t >= 0) {
            const auto& hit = hits[t];
            const int& r = hit[0], c = hit[1];
            int preRoof = dsu.top();
            if (grid[r][c] == -1)
                grid[r][c] = 0;
            else {
                for (const auto& dir: dirs) {
                    int nr = r + dir[0], nc = c + dir[1];
                    if (nc < 0 || nr < 0 || nr >= m || nc >= n || grid[nr][nc] != 1)
                        continue;
                    dsu.join(r*n+c, nr*n+nc);
                }
                if (r == 0)
                    dsu.join(r*n+c, m*n);
                grid[r][c] = 1;
                // cout << "top:" << dsu.top() << ", preRoof:" << preRoof << endl;
                res[t] = max(0, dsu.top() - preRoof - 1);
            }
        }
        return res;
    }
};
```

# Reference
- https://leetcode.com/problems/bricks-falling-when-hit/solution/
