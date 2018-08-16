# graph algorithm
- edge representation
  - set of edges: linked list or array for endpoints of edges
  - adjacent matrix: 2-dimensional V-by-V bool array
  - adjacent list: vertex-indexed array of lists
- unidirected graphs
  - depth first search
  - breadth first search
  - connected components: DFS
- digraphs
  - topological sort
  - strong components
    - Kosaraju-Shararir algorithm, 2 DFS
    - [explanation in GeeksforGeeks](https://www.geeksforgeeks.org/strongly-connected-components/)
- MST(minimum spanning tree)
  - Kruskal's algorithm: O(ElogE)
    - consider edges in ascending order of weight
    - add next edge to tree unless doing so would create a cycle
  - Prim's algorithm: O(ElogV)
    - start with vertex 0 and greedily grow tree T
    - add to T the min weight edge with exactly one endpoint in T
    - repeat until V-1 edges
    - note: the time complexity depends on implmenetation
- Shortest Paths
  - Dijkstra's algorithm: nonnegative weights, O(ElogV)
  - Topological sort: no directed cycle, O(E+V)
  - Bellman-Ford algorithm: no negative cycles: O(EV)
- maximum flow = minimum cut
  - Ford-Fulkerson algorithm, [explanation](https://www.geeksforgeeks.org/ford-fulkerson-algorithm-for-maximum-flow-problem/)
```
start with 0 flow
while there exists an augmenting path:
	- find an augmenting path
	- compute bottleneck capacity
	- increase flow on that path by bottleneck capacity
```
