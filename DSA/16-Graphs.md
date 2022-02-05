# Graphs

## Table of Content
- [What are Graphs?](#what-are-graphs)
  - [Terminology](#terminology)
  - [Types of Graphs](#types-of-graphs)
  - [Some Uses for Graphs](#some-uses-for-graphs)
- [Storing Graphs](#storing-graphs)
  - [Adjacency Matrix](#adjacency-matrix)
  - [Adjacency List](#adjacency-list)

## What are Graphs?
- A data structure that consists of nodes and connections between those nodes, where there is no specific pattern.
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




