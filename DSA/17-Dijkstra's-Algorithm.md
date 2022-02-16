# Dijkstra's Algorithm

## Table of Content
- [What is Dijkstra's Algorithm](#what-is-dijkstras-algorithm)
- [Dijkstra's Algorithm Flow](#dijkstras-algorithm-flow)
- [Essential Data Structures for Dijkstra's Algorithm](#essential-data-structures-for-dijkstras-algorithm)
  - [Weighted Graph](#weighted-graph)
  - [Simple Priority Queue using an Array](#simple-priority-queue-using-an-array)
- [Dijkstra's Algorithm Implementation](#dijkstras-algorithm-implementation)

## What is Dijkstra's Algorithm
- **An algorithm that finds the shortest path between two vertices on a (weighted) graph.**
### Use Cases
- GPS - find the fastest route.
- Network Routing - find open shortest path for data.
- Airline Tickets - find the cheapest route to your destination.
- Biology - model the spread of viruses among humans.

## Dijkstra's Algorithm Flow
1. Look to visit a new node. Pick the node with the shortest known distance to visit first.
2. Once we've moved to that node, look at each of its neighbors.
3. For each neighboring node, calculate the distance from the starting node by summing the total edges that lead to the node we're checking.
4. If the new total distance to a node is less than the previous total, store the new shorter distance for that node.

## Example
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/dijkstra-flow-1.png" alt="Dijkstra Flow 1" width="45%" />
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/dijkstra-flow-2.png" alt="Dijkstra Flow 2" width="45%" />
</p>

- `Previous` represents the current shortest path to any given node from `A`.
- Every time through, pick the current shortest distance from `A` that hasn't been visited. Explore each of its neighbors and calculate the new shortest distance to each neighbor. If it is shorter than what was stored, update what was stored and reflect the change on the previous node tracker.
- To get to `E` from `A`, we will go through the paths `A -> B -> E` (7), `A -> C -> D -> E` (7), `A -> C -> D -> F -> E` (6) and `A -> C -> F -> E` (7) and ultimately record the shortest one.

## Essential Data Structures for Dijkstra's Algorithm
### Weighted Graph
```js
class WeightedGraph {
  constructor() {
    this.adjacencyList = {};
  }
  
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }
  
  addEdge(vertex1, vertex2, weight) {
    this.adjacencyList[vertex1].push({ vertex: vertex2, weight });
    this.adjacencyList[vertex2].push({ vertex: vertex1, weight });
  }
}
```
### Simple Priority Queue using an Array
- Used to give us the next node to visit.
- For example, we want to `dequeue` the vertex with the shortest distance from `A` currently.
```js
class PriorityQueue {
  constructor() {
    this.values = [];
  }
  
  enqueue(val, priority) {
    // Assign a priority for each val.
    this.values.push({val, priority});
    // Sort the values based on priority.
    this.sort();
  }
  
  dequeue() {
    return this.values.shift();
  }
  
  // O(n * log n)
  sort() {
    this.values.sort((a, b) => a.priority - b.priority);
  }
}
```

## Dijkstra's Algorithm Implementation
- Define it as a function in the weighted graph class.
- It should accept a starting and ending vertex (ex: `A` to `E`).
- Create a few data structures.
  - Hash Map called `distancesFromStart` where for each vertex, we're going to store the current shortest distance from the starting vertex to the vertex. Initialize each vertex/key's value to `Infinity` except for the starting vertex initialized to `0`.
  - Priority Queue where each vertex has a priority representing the distance from the starting vertex. Initialize all vertices' priorities to be `Infinity` except for the starting vertex with `0`.
  - Hash Map called `previousVertices` where for each vertex, we're going to record the vertex that comes before it, which yields a shorter distance to it. Initialize all values to `null`.
- Loop as long as there is anything in the Priority Queue.
  - Dequeue a vertex from the priority queue.
  - If that vertex is the same as the ending vertex, end algorithm.
  - Else, loop through each of the vertex's neighbors.
    - Calculate the distance to the neighbor from the starting vertex.
    - If the distance is less than what is currently stored in `distances`.
      - Update `distancesFromStart` with the new lower distance.
      - Update `previousVertices` to reflect where we came from.
      - Enqueue the neighbor with the new total distance from the starting vertex.
```js
class WeightedGraph {
  constructor() {
    this.adjacencyList = {};
  }
  
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }
  
  addEdge(vertex1, vertex2, weight) {
    this.adjacencyList[vertex1].push({ vertex: vertex2, weight });
    this.adjacencyList[vertex2].push({ vertex: vertex1, weight });
  }
  
  Dijkstra(start, finish) {
    const vertices = new PriorityQueue();
    const distancesFromStart = {};
    const previousVertices = {};
    let path = [];
    let smallest;
    
    // Build up initial state for distancesFromStart, vertices and previousNodes.
    for (let vertex in this.adjacencyList) {
      if (vertex === start) {
        distancesFromStart[vertex] = 0;
        vertices.enqueue(vertex, 0);
      } else {
        distancesFromStart[vertex] = Infinity;
        vertices.enqueue(vertex, Infinity);
      }
      previousVertices[vertex] = null;
    }
    
    // While the priority queue is not empty.
    while (vertices.values.length) {
      smallest = vertices.dequeue().val;
      if (smallest === finish) {
        // Build up path to return.
        while (previousVertices[smallest]) {
          path.push(smallest);
          smallest = previousVertices[smallest];
        }
        break;
      }
      
      if (smallest || distancesFromStart[smallest] !== Infinity) {
        for (let neighbor in this.adjacencyList[smallest]) {
          // Find neighboring vertex.
          let nextVertex = this.adjacencyList[smallest][neighbor];
          // Calculate new distance to neighboring vertex (current distance for our vertex + distance to the neighbor).
          let candidate = distancesFromStart[smallest] + nextVertex.weight;
          let nextNeighbor = nextVertex.vertex;
          if (candidate < distancesFromStart[nextNeighbor]) {
            // Update new smallest distance to neighbor.
            distancesFromStart[nextNeighbor] = candidate;
            // Update how we got to neighbor.
            previousVertices[nextNeighbor] = smallest;
            // Enqueue in priority queue with new priority.
            vertices.enqueue(nextNeighbor, candidate);
          }
        }
      }
    }
    
    return path.concat(smallest).reverse();
  }
}
```




