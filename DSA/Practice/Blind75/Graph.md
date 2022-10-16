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

## 2. [Course Schedule](https://leetcode.com/problems/course-schedule/)
- Create an adjacency list.
- Use DFS to check each course for any cycles.
  - If there is a cycle for any course, not all courses are finishable.
- How to detect cycle for a course.
  - If we have to visit an already visited course for this DFS path.
- Make sure to "unvisit" the course since we are reusing it for another course's cycle check.
- Empty a course's prerequisites list if it checked out to not have any cycles (is finishable) to prevent from having to check again.
```js
// TC: O(V+E), SC: O(V+E)

const canFinish = (numCourses, prerequisites) => {
  const adjList = {};
  for (let i = 0; i < numCourses; i++) adjList[i] = [];
  for (let [course, prerequisite] of prerequisites) adjList[course].push(prerequisite);
  
  const visited = new Set();
  const dfs = (course) => {
    // Base Case: if this course has already been visited.
    if (visited.has(course)) return false;
    
    visited.add(course);
    for (let prerequisite of adjList[course]) {
      if (dfs(prerequisite) === false) return false;
    }
    visited.delete(course);
    adjList[course] = [];
    return true;
  }
  
  // Check each course if it has any cycles.
  for (let course in adjList) {
    if (dfs(course) === false) return false;
  }
  
  return true;
}
```
