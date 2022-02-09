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
  
  const visited = new Map();
  
  function dfs(node) {
    // Base Case: if the node has already been visited, return its clone.
    if (visited.has(node)) return visited.get(node);
    
    // Else, cloning process.
    else {
      const clone = new Node(node.val);
      visited.set(node, clone);
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






