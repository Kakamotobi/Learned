# Graphs - Easy

## 1791. Find Center of Star Graph
- Description
  - You are given an undirected star graph consisting of `n` nodes labeled from `1` to `n`.
  - A star graph is a graph where there is one center node and exactly `n-1` edges that connect the center node with every other node.
- Input: an integer array `edges` where each `edges[i] = [u,v]` indicates that there is an edge between nodes `u` and `v`.
- Output: return the center of the given star graph.
- Constraints
  - `3 <= n <= 105`
  - `edges.length == n - 1`
  - `edges[i].length == 2`
  - `1 <= ui, vi <= n`
  - `ui != vi`
  - The given edges represent a valid star graph.
### Example
```js
findCenter([[1,2],[2,3],[4,2]]); // 2
findCenter([[1,2],[5,1],[1,3],[1,4]]); // 1
```
### Solution 1
- Since the center node is connected with every other node (i.e., max depth is 1 from the center node), the center node must appear in every connection listed in `edges`.
```js
const findCenter = (edges) => {
  return edges[0][0] === edges[1][0] || edges[0][0] === edges[1][1] ? edges[0][0] : edges[0][1];
}
```
### Solution 2
- Since the center node is the only node with a larger number of edges than the rest of the nodes, create an adjacency list reprsenting edges and find the node with the most edges.

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

## 997. Find the Town Judge
- Description
  - Among people labeled `1` to `n`, there may be a town judge.
  - The town judge does not trust anybody, while is trusted by everyone (excl. self).
  - There may only be one town judge.
- Inputs
  - Integer `n` representing `n` people labeled from `1` to `n`.
  - Array `trust` where `trust[i] = [a,b]` represents that the person labeled `a` trusts the person labeled `b`.
- Output: return the label of the town judge if the town judge exists and can be identified, or return `-1` otherwise.
- Constraints
  - `1 <= n <= 1000`.
  - `0 <= trust.length <= 104`.
  - `trust[i].length == 2`.
  - All the pairs of trust are unique.
  - `ai != bi`.
  - `1 <= ai, bi <= n`.
### Example
```js
findJudge(3, [[1,3],[2,3]]) // 3
findJudge(3, [[1,3],[2,3],[3,1]]) // -1
findJudge(3, [[1,2],[2,3]]) // -1
```
### Solution
- Since this is a directed graph where for `[a,b]`, `a` trusts `b` but not vice versa, track counts using an array instead of a hash map.
  - If using a hash map, it is not feasible to simultaneously (1) find the unrecorded vertex (town judge), and (2) check whether the unrecorded vertex (town judge) is trusted by all.
- Use the array's indices as vertex labels. Update each vertex's value (representing the number of people that trust that vertex) by increment/decrement.
  - Decrement for `a` since it cannot be the town judge as it trusts `b`.
  - Increment for `b` since it can possibly be the town judge.
- The vertex(index) with `n-1` as its value is the town judge.
```js
const findJudge = (n, trust) => {
  const numOfTrusters = new Array(n+1).fill(0);
  for (const [a,b] of trust) {
    // We don't really care for the end value of a. We just need to make sure that it is not a potential town judge.
    numOfTrusters[a]--;
    // b is a potential town judge.
    numOfTrusters[b]++;
  }
  
  for (let i = 1; i < numOfTrusters.length; i++) {
    if (numOfTrusters[i] === n - 1) return i;
  }
  
  return -1;
}
```

## 463. Island Perimeter
- **Input:** an array of arrays `grid` representing a map where `grid[i][j] = 1` represents land and `grid[i][j] = 0` represents water.
- **Description**
  - Grid cells are connected horizontally/vertically (not diagonally). The `grid` is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).
  - The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1.
- **Output:** return the perimeter of the island.
### Example
```js
islandPerimeter([[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]); // 16
```
```js
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]
```
### Solution
- Find a block of the island and traverse through the lands to accumulate the perimeter.
```js
const islandPerimeter = (grid) => {
  let perimeter = 0;
  
  const moves = [[-1,0],[0,1],[1,0],[0,-1]];
  const visited = {};
  const dfs = (x, y) => {
    // If coord is out of bounds OR coord is water.
    if (x <= 0 || x >= grid.length || y <= 0 || y >= grid[0].length || grid[x][y] === 0) {
      perimeter++;
      return;
    }
    // If coord has already been visited, return.
    if (visited[x]?.includes(y)) return;
    
    // Record coord as visited.
    if (visited[x]) visited[x].push(y);
    else visited[x] = [y];
    
    for (let move of moves) {
      const newX = x + move[0];
      const newY = y + move[1];
      dfs(newX, newY);
    }
  }
  
  // Find a block of the island and dfs that block.
  for (let i = 0; i < grid.length; i++) {
    for (let (j = 0; j < grid[i].length; j++) {
      if (grid[i][j] === 1) {
        dfs(i, j);
        return perimeter;
      }
    }
  }
}
```
