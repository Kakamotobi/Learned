# Graphs Medium

## 133. Clone Graph
- Input: a reference of a node in a connected undirected graph.
- Output: return the copy of the given node as a reference to the cloned graph.
- Constraints
  - The number of nodes in the graph is in the range `[0, 100]`.
  - `1 <= Node.val <= 100`
  - `Node.val` is unique for each node.
  - There are no repeated edges and no self-loops in the graph.
  - The Graph is connected and all nodes can be visited starting from the given node.
### Solution
- Use a hash map to map the old node to the cloned version (keep track of visited nodes).
- Depth-first traverse recursively.
  - Base Case: if the current node has already been visited, return its clone version.
  - Else create its clone, map it to the clone, then make clones of its neighbors.
  - Return the clone.
```js
const cloneGraph = (node) => {
  if (!node) return;
  
  const nodesToClones = new Map();
  
  function dfs(node) {
    // Base Case: if the node has already been visited, return its clone.
    if (nodesToClones.has(node)) return nodesToClones.get(node);
    
    // Else, cloning process.
    else {
      const clone = new Node(node.val);
      nodesToClones.set(node, clone);
      for (let neighbor of node.neighbors) {
        clone.neighbors.push(dfs(neighbor));
      }
      return clone;
    }
  }
  
  return dfs(node);
}
```
#### Flow Example
```js
// Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
// 1 -- 2
// |    |
// 4 -- 3

// dfs({val: 1, neighbors: [2,4]}) // return node 1 // (9)
   // dfs({val: 2, neighbors: [1,3]}) // return node 2 as 1's neighbor // (7)
      // dfs({val: 1, neighbors: [2,4]}) // return node 1 as 2's neighbor // (1)
      // dfs({val: 3, neighbors: [2,4]}) // return node 3 as 2's neighbor // (6)
         // dfs({val: 2, neighbors: [1,3]}) // return node 2 as 3's neighbor // (2)
         // dfs({val: 4, neighbors: [1,3]}) // return node 4 as 3's neighbor // (5)
            // dfs({val: 1, neighbors: [2,4]}) // return node 1 as 4's neighbor // (3)
            // dfs({val: 3, neighbors: [2,4]}) // return node 3 as 4's neighbor // (4)
   // dfs({val: 4, neighbors: [1,3]}) // return node 4 as 1's neighbor // (8)
```

## 207. Course Schedule
- Input: 
  - Integer `numCourses` representing the total number of courses labeled from `0` to `numCourses-`.
  - Array `prerequisites` representing `prerequisites[i] = [a,b]` where you must take course `b` first if you want to take course `a`.
- Output: return `true` if you can finish all courses.
- Constraints
  - `1 <= numCourses <= 105`.
  - `0 <= prerequisites.length <= 5000`.
  - `prerequisites[i].length == 2`.
  - `0 <= ai, bi < numCourses`.
  - All the pairs `prerequisites[i]` are unique.
### Example
```js
canFinish(2, [[0,1]); // true
canFinish(2, [[0,1],[1,0]]); // false
canFinish(3, [[0,1],[1,2],[2,0]]); // false
canFinish(4, [[0,1],[1,2],[2,3],[3,1]]); // false;
```
### Solution
- Create an adjacency list representing `prerequisites`. Include empty arrays.
- Create a set to keep track of visited nodes.
- Run dfs on each node (recursively call dfs on its prerequisites).
  - Base Cases:
    - If this node's prerequisite list is empty, return true.
    - If this node has already been visited (node is in a loop), return false.
  - Record node as visited.
  - Run dfs on each of this node's prerequisites.
    - If a prerequisite returned false (there is a loop), return false.
  - Else
    - Remove node from visited for backtracking.
    - Empty this node's prerequisites list since there is no loop.
    - Return true.
```js
const canFinish = (numCourses, prerequisites) => {
  const adjcencyList = {};
  // Initialize adjacency list with empty arrays.
  for (let i = 0; i < numCourses; i++) {
    adjacencyList[i] = [];
  }
  // Fill up adjacency list.
  for (const [a,b] of prerequisites) {
    adjacencyList[a].push(b);
  }
  
  const visited = new Set();
  
  function dfs(course) {
    if (visited.has(course)) return false;
    if (!adjacencyList[course]) return true;
    
    visited.add(course);
    for (let prerequisite of adjacencyList[course]) {
      if (dfs(prerequisite) === false) return false;
    }
    // At this point, there is no longer the need to consider this course's prerequisites since they have been verified to be true.
    adjacencyList[course] = [];
    // Remove this course from visited since we are backtracking recursively.
    visited.delete(course);
    return true;
  }
  
  // Run dfs on each course.
  for (let course in adjacencyList) {
    if (dfs(course) === false) return false;
  }
  return true;
}
```

## 797. All Paths from Source to Target
- Input: an array `graph` where `graph[i]` is a list of nodes that node `i` has a directed edge towards.
- Output: return all possible paths from node `0` to node `n-1`.
- Constraints
  - `n == graph.length`.
  - `2 <= n <= 15`.
  - `0 <= graph[i][j] < n`.
  - `graph[i][j] != i` (i.e., there will be no self-loops).
  - All the elements of `graph[i]` are unique.
  - The input graph is guaranteed to be a Directed Acyclic Graph.
### Example
```js
allPathsSourcetoTarget([[1,2],[3],[3],[]]) // [[0,1,3],[0,2,3]]
allPathsSourcetoTarget([[4,3,1],[3,2,4],[3],[4],[]]) // [[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
```
### Solution
- `graph` is an adjacency list.
- DFS traverse from node `0`.
  - Need a way to keep track of the current path.
  - If the current path reaches the last node, record the current path as a possible path.
  - Backtrack and try other paths.
```js
const allPathsSourcetoTarget = (graph) => {
  const possiblePaths = [];
  
  const lastNode = graph.length - 1;
  
  function dfs(node, currentPath) {
    currentPath.push(node);
    // Base Case
    if (node === lastNode) {
      possiblePaths.push(currentPath);
      return;
    }
    
    for (let neighbor of graph[node]) {
      dfs(neighbor, [...currentPath];
    }
  }
  
  dfs(0, []);
  
  return possiblePaths;
}
```
