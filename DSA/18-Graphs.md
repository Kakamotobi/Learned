# Graphs

## Table of Content
- [What are Graphs?](#what-are-graphs)
  - [Terminology](#terminology)
  - [Types of Graphs](#types-of-graphs)
  - [Some Uses for Graphs](#some-uses-for-graphs)
- [Storing Graphs](#storing-graphs)
  - [Adjacency Matrix](#adjacency-matrix)
  - [Adjacency List](#adjacency-list)
- [Graph Traversal](#graph-traversal)
  - [Graph Traversal Uses](#graph-traversal-uses)
  - [Depth-First Traversal](#depth-first-traversal)
  - [Breadth-First Traversal](#breadth-first-traversal)
- [Undirected Graph Im  plementation - using adjacency list](#undirected-graph-implementation---using-adjacency-list)

## What are Graphs?
- A data structure that consists of nodes and connections between those nodes, where there is no specific pattern or root.
- Below: examples of graphs.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/graph-example-1.png" alt="Graph Example 1" width="40%" />
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/graph-example-2.png" alt="Graph Example 2" width="40%" />
</p>

### Terminology
- **Vertex:** a node.
- **Edge:** connection between nodes.
- **Weighted/Unweighted:** whether values are assigned to distances between vertices.
- **Directed/Undirected:** whether directions are assigned to distances between vertices.
### Types of Graphs
- **Tree**
  - A tree is an undirected graph in which any two vertices are connected by exactly one path.
- **Undirected Graph**
  - There is no direction to the edges (i.e. they are two-way connections).
  - Ex: Facebook user account (friends).

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/graph-example-1.png" alt="Undirected Graph Example" width="40%" />
</p>

- **Directed Graph**
  - Edges have a direction (i.e. they are one-way connections).
  - Ex: Instagram user account (followers).

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/directed-graph.png" alt="Directed Graph Example" width="40%" />
</p>

- **Unweighted Graph**
  - Edges do not have values associated with them.
  - Ex: college courses and their prerequisites.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/graph-example-2.png" alt="Unweighted Graph Example" width="40%" />
</p>

- **Weighted Graph**
  - Edges have values (there is info about the connection itself).
  - Ex: distance between point A to point B in a map.
  - Ex: user's "appetite" for Kanye West (based on the number of Kanye West posts that the user liked, etc).

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/weighted-graph.png" alt="Unweighted Graph Example" width="40%" />
</p>

### Some Uses for Graphs
- Social Networks
- Location/Mapping/Directions
  - Ex: finding the shortest path taking into account the length of the road, speed limit, bad terrain, traffic, closed, etc.
- Recommendation Engines
  - Ex: "people also watched", "you might also like...", "people you might know", "frequently bought with"
- Routing Algorithms
- Visual Hierarchy
- File System Optimizations

## Storing Graphs
- Need a way to store the vertices and their edges.
- The two standard approaches to do this are: adjacency matrix and adjacency list.
  - Typically use an adjacency list because most data in the real-world tends to be sparsed. There can be lots of vertices. However, they are usually not all connected.
    - i.e., there aren't many connections compared to how many there could be.
### Adjacency Matrix
- **Matrix:** a two-dimensional structure usually implemented with nested arrays to store information in rows and columns.
#### Pros and Cons
- Takes up more space (in sparse graphs).
- Is slower to iterate over all edges.
- However, is faster to lookup a specific edge.
#### Adjacency Matrix Big O
- **|V|**: number of vertices
- **|E|**: number of edges
- **Add Vertex:** O(|V<sup>2</sup>|)
- **Add Edge:** O(1)
- **Remove Vertex:** O(|V<sup>2</sup>|)
- **Remove Edge:** O(|1|)
- **Query:** O(|1|)
- **Storage:** O(|V<sup>2</sup>|)
  - As a 2D structure, adding a new vertex means adding an entire row and column to the matrix.

<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/adjacency-matrix.png" alt="Adjacency Matrix" width="60%" />
</p>

### Adjacency List
- Use an array/list or hashtable to store the edges.
#### Pros and Cons
- Can take up less space (in sparse graphs).
- Is faster to iterate over all edges.
- However, can be slower to lookup a specific edge.
#### Adjacency List Big O
- **|V|**: number of vertices
- **|E|**: number of edges
- **Add Vertex:** O(1)
- **Add Edge:** O(1)
- **Remove Vertex:** O(|V| + |E|)
- **Remove Edge:** O(|E|)
- **Query:** O(|V| + |E|)
- **Storage:** O(|V| + |E|)
  - The amount of storage is determined by the amount of edges/connections there are.
#### Using an Array
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/adjacency-list-numeric.png" alt="Adjacency List Numeric" width="60%" />
</p>

- Ex: index `3` of the array contains an array of the connecting edges from the vertex `3`.
#### Using a Hash Table
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/adjacency-list-string.png" alt="Adjacency List String" width="60%" />
</p>

- Ex: the key `A` contains an array of the connecting edges from the vertex `A`.

## Graph Traversal
### Graph Traversal Uses
- Peer to peer networking.
- Web crawlers.
- Finding "closest" matches/recommendations.
- Shortest path problems.
  - GPS navigation
  - Solving mazes.
  - AI (a graph can be used to store different moves. Then traverse it to find the shortest path to win the game).
### Depth-First Traversal
- Explore as far as possible down one branch before "backtracking".
- For graphs, depth-first traversal means following the neighbors before visiting siblings.
- DFS is often preferred to visit every node as it is a bit simpler.
- Keep track of vertices that have already been "visited", effectively "crossing it off" from other vertices' list of connections in order to prevent possible infinite loops.
  - Ex: once vertex `A` has been visited, cross it off from `B` and `C`'s list of connections by using a hash table as an auxiliary structure.
#### Example
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/depth-first-graph-traversal.png" alt="Depth-First Graph Traversal" width="60%" />
</p>

- Depth-first traversal starting from `A` will result to a traversal of `["A","B","D","E","C","F"]` when using the recursive approach and `["A","C","E","F","D","B"]` when using the iterative approach.
### Breadth-First Traversal
- Explore all neighbors at current depth first before moving on down the branch.
- BFS is generally better in finding the shortest path (or just any path) between two nodes, since it doesn't have to go all the way down a path before moving on to the next path.
#### Example
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/breadth-first-graph-traversal.png" alt="Breadth-First Graph Traversal" width="60%" />
</p>

- Breadth-first traversal starting from `A` will result to a traversal of `["A","B","C","D","E","F"]`.
### Bidirectional Search
- Used to find the shortest path between two nodes.
- Two BFS is simultaneously executed (one from each node). If the two searches collide, there is a path.
- This is faster than a single BFS to find the shortest path between two nodes.

## Undirected Graph Implementation - using adjacency list
```js
class UndirectedGraph {
  constructor() {
    this.adjacencyList = {};
  }
  
  // Add a new vertex to the graph.
  addVertex(vertex) {
    // If it doesn't exist already, set the vertex to be a key on the adjacency list and set its value to be an empty array.
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }
  
  // Add an edge between two vertices.
  addEdge(vertex1, vertex2) {
    // If the vertices are valid.
    if (this.adjacencyList[vertex1] && this.adjacencyList[vertex2]) {
      // Find the key of vertex1 and push vertex2 to the array, and vice versa.
      this.adjacencyList[vertex1].push(vertex2);
      this.adjacencyList[vertex2].push(vertex1);
    }
  }
  
  // Remove an edge between two vertices.
  removeEdge(vertex1, vertex2) {
    // If the vertices are valid.
    if (this.adjacencyList[vertex1] && this.adjacencyList[vertex2]) {
      // Find the key of vertex1 and remove vertex2 to the array, and vice versa.
      this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(v => v !== vertex2);
      this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(v => v !== vertex1);
    }
  }
  
  // Remove a vertex from the graph.
  removeVertex(vertex) {
    // Remove all values in the adjacency list for this vertex.
    for (let v of this.adjacencyList[vertex]) {
      this.removeEdge(vertex, v);
    }
    // Delete the key in the adjacency list for this vertex.
    delete this.adjacencyList[vertex];
  }
  
  // Depth-First Traversal Recursive
  DepthFirstTraversalRecursive(startingVertex) {
    // To account for `this`'s meaning changing in helper function.
    const adjacencyList = this.adjacencyList;
    // If starting vertex is invalid, return undefined.
    if (!adjacencyList[startingVertex]) return undefined;
    // Initialize array to store the visited node values.
    const visited = [];
    // Initialize an object to keep track of visited nodes.
    const tracker = {};
    // Recursive helper function.
    function traverse(vertex) {
      // Push vertex to visited array and record in tracker.
      visited.push(vertex);
      tracker[vertex] = true;
      // Loop over all neighbors/vertices in the adjacency list for that vertex.
      for (let neighbor of adjacencyList[vertex]) {
        // If the neighbor vertex has not been visited, recursive call on that vertex.
        if (!tracker[neighbor]) traverse(neighbor);
      }
    }
    traverse(startingVertex);
    return visited;
  }
  
  // Depth-First Traversal Iterative using Stack
  DepthFirstTraversalIterative(startingVertex) {
    if (!this.adjacencyList[startingVertex]) return undefined;
    const visited = [];
    const tracker = {};
    // Initialize stack with starting vertex.
    const stack = [startingVertex];
    // Variable for current vertex (top stack).
    let currVertex;
    // While the stack is not empty.
    while (stack.length) {
      // Pop from stack.
      currVertex = stack.pop();
      // If currVertex has not been visited yet.
      if (!tracker[currVertex]) {
        // Push currVertex to visited array and record in tracker.
        visited.push(currVertex);
        tracker[currVertex] = true;
        // Push currVertex's neighbors onto the stack.
        stack.push(...this.adjacencyList[currVertex]);
      }
    }
    return visited;
  }
  
  // Breadth-First Traversal using Queue
  BreadthFirstTraversal(startingVertex) {
    if (!this.adjacencyList[startingVertex]) return undefined;
    const visited = [];
    const tracker = {};
    // Initialize queue with starting vertex.
    const queue = [startingVertex];
    // Record in tracker.
    tracker[startingVertex] = true;
    // Variable for current vertex (first in queue).
    let currVertex;
    // While the queue is not empty.
    while (queue.length) {
      // Dequeue from queue.
      currVertex = queue.shift();
      // Push currVertex to visited array and record in tracker.
      visited.push(currVertex);      
      // Loop over each neighbor/vertex in the adjacency list for currVertex.
      for (let neighbor of this.adjacencyList[currVertex]) {
        // If neighbor has not been visited yet.
        if (!tracker[neighbor]) {
          // Enqueue neighbor.
          queue.push(neighbor);
          // Record in tracker.
          tracker[neighbor] = true;
        }
      }
    }
    return visited;
  }
}
```
