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
  for (let i = 0; i < numCourses; i++) { // use `numCourses` instead of the adjacency list to account for unconnected graphs.
    if (dfs(i) === false) return false;
  }
  
  return true;
}
```

## 3. [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)
### Solution 1
- Call DFS on every cell to check if it can reach both the Pacific Ocean and Atlantic Ocean.
- TC: O((m\*n)<sup>2</sup>)
  - m: number of rows.
  - n: number of columns.
### Solution 2
- For each ocean, we know that its bordering cells lead to that respective ocean.
  - So, start from the bordering cells of each ocean and use DFS to reach the highest height it can reach (record the path to that cell).
- The commonly shared cells are the cells where rain water can flow from it to both oceans.
```js
// TC: O(m * n), SC: O()

const pacificAtlantic = (heights) => {
  const result = [];

  const moves = [[-1,0],[0,1],[1,0],[0,-1]];
  
  const numRows = heights.length;
  const numCols = heights[0].length;
  
  const pacificVisited = new Set();
  const atlanticVisited = new Set();
  
  const dfs = (r, c, visited, prevHeight) => {
    // If cell has already been visited, OR
    // if cell is out of bounds, OR
    // if cell is not "reachable"
    if (visited.has(`[${r}, ${c}]`) ||
        r < 0 || r >= numRows || c < 0 || c >= numCols ||
        heights[r][c] < prevHeight
       ) return;
    
    visited.add(`[${r},${c}]`);
    
    for (let [x, y] of moves) {
      dfs(r + x, c + y, visited, heights[r][c]);
    }
  }
  
  // Horizontal Bordering Cells
  for (let c = 0; c < numCols; c++) {
    // Cells bordering Pacific Ocean
    dfs(0, c, pacificVisited, heights[0][c]);
    
    // Cells bordering Atlantic Ocean
    dfs(numRows - 1, c, atlanticVisited, heights[numRows - 1][c]);
  }
  
  // Vertical Bordering Cells
  for (let r = 0; r < numRows; r++) {
    // Cells bordering Pacific Ocean
    dfs(r, 0, pacificVisited, heights[r][0]);
    
    // Cells bordering Atlantic Ocean
    dfs(r, numCols - 1, atlanticVisited, heights[r][numCols - 1]);
  }
  
  // Check for overlapping cells.
  for (let r = 0; r < numRows; r++) {
    for (let c = 0; c < numCols; c++) {
      if (pacificVisited.has(`[${r}, ${c}]`) && atlanticVisited.has(`[${r}, ${c}]`)) result.push([r,c]);
    }
  }
  
  return result;
}
```

## 4. [Number of Islands](https://leetcode.com/problems/number-of-islands/)
- Loop over all cells in `grid`.
  - If cell is `"1"` and has not been visited, DFS (objective is to mark all connected `"1"` cells as visited).
    - Base Cases:
      - If cell is out of bounds OR cell is `"0"` OR cell has already been visited.
```js
// TC: O(m * n), SC: O(m * n)

const numIslands = (grid) => {
  let numIslands = 0;
  
  const moves = [[-1,0],[0,1],[1,0],[0,-1]];
  
  const m = grid.length;
  const n = grid[0].length;
  
  const visited = new Set();
  
  const dfs = (x, y) => {
    if (x < 0 || x >= m || y < 0 || y >= n ||
        grid[x][y] === "0" ||
        visited.has(`${x},${y}`)
       ) return;
    
    visited.add(`${x},${y}`);
    
    for (let [mx, my] of moves) {
      dfs(x + mx, y + my);
    }
  }
  
  for (let x = 0; x < m; x++) {
    for (let y = 0; y < n; y++) {
      if (grid[x][y] === "1" && !visited.has(`${x},${y}`)) {
        dfs(x, y);
        numIslands++;
      }
    }
  }
  
  return numIslands;
}
```
