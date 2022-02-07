# Graphs - Easy

## 1971. Find if Path Exists in Graph
- Inputs
  - Integer `n` representing the number of vertices. 
  - Integer array `edges` where each `edges[i] = [u,v]` denotes a bi-directional edge between vertices `u` and `v`.
  - Integer `source` representing the starting vertex.
  - Integer `destination` representing the ending vertex.
- Output: return `true` if there is a valid path from vertex `source` to vertex `destination`. Otherwise, return `false`.
- Constraints
  - `1 <= n <= 2 * 105`
  - `0 <= edges.length <= 2 * 105`
  - `edges[i].length == 2`
  - `0 <= ui, vi <= n - 1`
  - `ui != vi`
  - `0 <= source, destination <= n - 1`
  - There are no duplicate edges.
  - There are no self edges.
### Example
```js
validPath(3, [[0,1],[1,2],[2,0]], 0, 2) // true
validPath(6, [[0,1],[0,2],[3,5],[5,4],[4,3]], 0, 5) // false
```
### Solution
- Idea: completely traverse the whole graph.
- Create an adjacency list that represents `edges`.
- Depth-first traverse the graph starting from `source` using the adjacency list and accumulate the visited vertices.
- If `destination` is among the visited vertices, return `true`, else return `false`.
```js
const validPath = (n, edges, source, destination) => {
  const adjacencyList = {};
  for (const [u, v] of edges) {
    if (adjacencyList[u]) adjacencyList[u].push(v);
    else adjacencyList[u] = [v];
    if (adjacencyList[v]) adjacencyList[v].push(u);
    else adjacencyList[v] = [u];
  }
  
  const visited = [];
  const tracker = {};
  function dfs(vertex) {
    visited.push(vertex);
    tracker[vertex] = true;
    if (adjacencyList[vertex]) {
      for (let neighbor of adjacencyList[vertex]) {
        if (!tracker[neighbor]) dfs(neighbor);
      }
    }
  }
  dfs(source);
  
  return visited.includes(destination);
}
```
- Could be optimized by using Set instead of an array for `visited` (quicker look up time).
