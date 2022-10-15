# Graph

## 1. [Clone Graph](https://leetcode.com/problems/clone-graph/)
- DFS to traverse through each node in original graph.
- Create a new node for each node, and mark as visited.
```js
// TC: O(V+E), SC: O(V+E)

/*
function Node(val, neighbors) {
  this.val = val === undefined ? 0 : val;
  this.neighbors = neighbors === undefined ? [] : neighbors;
}
*/

const cloneGraph = (node) => {
  if (!node) return;
  
  // Original nodes are mapped to cloned nodes.
  const visited = new Map();
  
  const dfs = (node) => {
    // Base Case: if node has already been visited, return its clone.
    if (visited.has(node)) return visited.get(node);
    
    const clone = new Node(node.val, []);
    visited.set(node, clone);
    for (let neighbor of node.neighbors) {
      clone.neighbor.push(dfs(neighbor));
    }
    
    return clone;
  }
  
  return dfs(node);
}
```
