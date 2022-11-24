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
- [Undirected Graph Implementation - using adjacency list](#undirected-graph-implementation---using-adjacency-list)
- [Topological Sorting for Directed Acyclic Graphs](#topological-sorting-for-directed-acyclic-graphs)

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
- **Neighbors:** the nodes that are connected to the given node.
- **Degree:** the number of edges (i.e. neighbors) connected to the given node.
- **Path:** a sequence of nodes connected by edges.
- **Path Length:** the number of edges in a path.
- **Cycle:** a path that starts and ends at the same node.
- **Connectivity**
  - Two nodes are **connected** if a path exists between them.
  - A graph is **connected** when all nodes are connected.
- **Connected Component:** a subset of nodes of a graph that is connected.
- **Weighted/Unweighted:** whether values are assigned to distances between vertices.
- **Directed/Undirected:** whether directions are assigned to distances between vertices.
  - Directed (Cyclic) Graph vs. Directed Acyclic Graph (DAG)
### Types of Graphs
- **Tree**
  - A tree is an undirected graph in which any two vertices are connected by exactly one path.
  - Conditions
    - Trees are connected and acyclic.
    - Removing an edge disconnects the tree.
    - Adding an edge creates a cycle.
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

- **Bipartite Graph**
  - A graph *G = (V,E)* is bipartite if *V* can be partitioned into sets *X,Y* such that EVERY EDGE has one end in *X* and one in *Y*.
  - Ex: client-server connections, student-college stable matching graph.
  - Any graph containing an odd cycle (odd number of nodes) is not a bipartite.
  - Bipartite Testing
    - Nodes in the same layer should not have any edges between them.
    - Algorithm
      - Run BFS on a node.
      - If there is an edge between two nodes in the same layer, the graph is not a bipartite.
      - Continue with BFS.
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
- *Preorder DFS records a node as it is encountered.*
- *Postorder DFS records a node only after it has no more neighbors to explore (dead-end).*
#### Example
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/depth-first-graph-traversal.png" alt="Depth-First Graph Traversal" width="60%" />
</p>

- Depth-first traversal starting from `A` will result to a traversal of `["A","B","D","E","C","F"]` when using the recursive approach and `["A","C","E","F","D","B"]` when using the iterative approach.
#### DFS Applications
- Cycle Detection
  - If you visit a node that has already been visited, there is a cycle.
- Finding Connected Components of a Graph
  - Run DFS on a random node to find a connected component in the graph.
  - Then, run DFS on an unvisited node to find another connected component.
- Topological Sort
  - Finding a valid order to execute tasks in a Directed Acyclic Graph.
  - If node `A` points to node `B`, node `A` must be done before node `B`.
  - Reversing a DFS Postorder traversal of a DAG will always return a valid Topological Sort of it.
- Maze Generation
  - Instead of inserting neighbor nodes in order, randomly insert them into the stack.
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
    // Remove all edges for this vertex.
    for (let v of this.adjacencyList[vertex]) {
      this.removeEdge(vertex, v);
    }
    // Delete this vertex from the graph.
    delete this.adjacencyList[vertex];
  }
  
  // Depth-First Traversal Recursive
  DepthFirstTraversalRecursive(startingVertex) {
    const adjacencyList = this.adjacencyList;
    
    // If starting vertex is invalid, return.
    if (!adjacencyList[startingVertex]) return undefined;
    
    // Array to store the traversal path.
    const path = [];
    // Hash Set to keep track of visited vertices.
    const visited = new Set();
    
    // Recursive helper function.
    function traverse(vertex) {
      // Push vertex to path array and record as visited.
      path.push(vertex);
      visited.add(vertex);
      // Loop over all neighbors in the adjacency list for this vertex.
      for (let neighbor of adjacencyList[vertex]) {
        // If the neighbor vertex has not been visited yet, recursive call on that neighbor.
        if (!visited.has(neighbor)) traverse(neighbor);
      }
    }
    
    traverse(startingVertex);
    
    return path;
  }
  
  // Depth-First Traversal Iterative using Stack
  DepthFirstTraversalIterative(startingVertex) {
    if (!this.adjacencyList[startingVertex]) return undefined;
    
    const path = [];
    const visited = new Set();
    
    // Initialize stack with starting vertex.
    const stack = [startingVertex];
    
    // Variable for current vertex (top stack).
    let currVertex;
    
    // While the stack is not empty.
    while (stack.length) {
      currVertex = stack.pop();
      
      if (!visited.has(currVertex)) {
        path.push(currVertex);
        visited.add(currVertex);
        stack.push(...this.adjacencyList[currVertex]);
      }
    }
    
    return path;
  }
  
  // Breadth-First Traversal using Queue
  BreadthFirstTraversal(startingVertex) {
    if (!this.adjacencyList[startingVertex]) return undefined;
    
    const path = [];
    const visited = new Set();
    
    // Initialize queue with starting vertex.
    const queue = [startingVertex];
    
    // Record startingVertex as visited.
    visited.add(startingVertex);
    
    // Variable for current vertex (first in queue).
    let currVertex;
    
    // While the queue is not empty.
    while (queue.length) {
      currVertex = queue.shift();

      path.push(currVertex);
      
      for (let neighbor of this.adjacencyList[currVertex]) {
        if (!tracker.has(neighbor)) {
          queue.push(neighbor);
          visited.add(neighbor);
        }
      }
    }
    
    return path;
  }
}
```

## Topological Sorting for Directed Acyclic Graphs
- A topological sorting is an ordering of the nodes in a **directed acyclic graph** such that all edges go "forward" in the ordering,
  - i.e. returns an array of the nodes where each node appears before all the nodes that it points to.
- A graph can have more than one valid topological ordering.
### Algorithm
- The idea is that a DAG always has a node with no incoming edges, and removing that node always produces a new DAG.
  - Any node with no incoming edges is valid to be first in topological sorting.
- Algorithm - TC: O(n^2 + m)
  - While there are nodes remaining
    - Find a node that has no incoming edges
    - Place that node in the order
    - Remove that node and all of its outgoing edges from the graph
- Algorithm - TC: O(m + n)
  - Same as above but instead of searching the whole graph every time for a node that has no incoming edges, maintain an incoming edge count for each node (Set up requires O(m + n), update requires O(1)). When a node's count becomes 0, add it to the order.
