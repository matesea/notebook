# problem
Given an m x n matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.

Note:
Both m and n are less than 110. The height of each unit cell is greater than 0 and is less than 20,000.

Example:
```C++
Given the following 3x6 height map:
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]

Return 4.
```

refer to [problem description](https://leetcode.com/problems/trapping-rain-water-ii/description/) for visualized image.

# analysis
Consider the solution for 1-D case below and extend it to resolve 2-D case.
```Java
public int trap(int[] height) {
    int res = 0, l = 0, r = height.length - 1;
        
    while (l < r) {
        if (height[l] <= height[r]) {
            if (l + 1 < r) {
                res += Math.max(0, height[l] - height[l + 1]);
                height[l + 1] = Math.max(height[l], height[l + 1]);
            }
                
            l++;
                
        } else {
            if (l < r - 1) {
                res += Math.max(0, height[r] - height[r - 1]);
                height[r - 1] = Math.max(height[r], height[r - 1]);
            }
                
            r--;
        }
    }
        
    return res;
}
```
2-D solution:
```Java
int[][] dirs = new int[][] {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
public int trapRainWater(int[][] heightMap) {
    int m = heightMap.length;
    int n = (m == 0 ? 0 : heightMap[0].length);
    int res = 0;
        
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[2] - b[2]);
    boolean[][] visited = new boolean[m][n];
        
    for (int i = 0; i < m; i++) {
        pq.offer(new int[] {i, 0, heightMap[i][0]});
        pq.offer(new int[] {i, n - 1, heightMap[i][n - 1]});
        visited[i][0] = visited[i][n - 1] = true;
    }
        
    for (int j = 1; j < n - 1; j++) {
        pq.offer(new int[] {0, j, heightMap[0][j]});
        pq.offer(new int[] {m - 1, j, heightMap[m - 1][j]});
        visited[0][j] = visited[m - 1][j] = true;
    }
        
    while (!pq.isEmpty()) {
        int[] cell = pq.poll();
        	
        for (int[] d : dirs) {
            int i = cell[0] + d[0], j = cell[1] + d[1];
            if (i < 0 || i >= m || j < 0 || j >= n || visited[i][j]) continue;
            res += Math.max(0, cell[2] - heightMap[i][j]);
            pq.offer(new int[] {i, j, Math.max(heightMap[i][j], cell[2])});
            visited[i][j] = true;
        }
    }
        
    return res;
}
```

# reference
https://leetcode.com/problems/trapping-rain-water-ii/discuss/89495/How-to-get-the-solution-to-2-D-"Trapping-Rain-Water"-problem-from-1-D-case
