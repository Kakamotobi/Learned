# Dijkstra's Algorithm

## Table of Content
- [What is Dijkstra's Algorithm](#what-is-dijkstras-algorithm)
- [Dijkstra's Algorithm Flow](#dijkstras-algorithm-flow)
- [Essential Data Structures for Dijkstra's Algorithm](#essential-data-structures-for-dijkstras-algorithm)
  - [Weighted Graph](#weighted-graph)
  - [Priority Queue](#priority-queue)
- [Dijkstra's Algorithm Implementation](#dijkstras-algorithm-implementation)
- [Reference](#reference)

## What is Dijkstra's Algorithm
- **An algorithm that finds the shortest path between two vertices in a (weighted) graph.**
- Dijkstra's algorithm is an application of greedy algorithm.
- Dijkstra's algorithm always successfully identifies the shortest path when the edges are all positive. However, the algorithm can fail when there are negative edges.
  - As a greedy approach, once a node has been visited (meaning that the algorithm found the shortest path to that node), it cannot be reconsidered even if there is another path with shorter distance.
  - This only becomes an issue if there are negative weights in the graph.
> The algorithm maintains a set of unvisited nodes and calculates a tentative distance from a given node to another. If the algorithm finds a shorter way to get to a given node, the path is updated to reflect the shorter distance. This problem has satisfactory optimization substructure since if A is connected to B, B is connected to C, and the path must go through A and B to get to the destination C, then the shortest path from A to B and the shortest path from B to C must be a part of the shortest path from A to C. | Brilliant

> Dijkstra's algorithm to find the shortest path between A and B picks the unvisited vertex with the lowest distance, calculates the distance through it to each unvisited neighbor, and updates the neighbor's distance if smaller. Mark visited (set to red) when done with neighbors. | Brilliant
### Pros and Cons
#### Pros
- It has a linear time complexity, making it easy to be used for large problems.
- It is useful in finding the shortest distance, hence can be used in maps and calculating traffic.
#### Cons
- It is unable to handle negative weights.
- There is some wastage of time since it follows a blind approach.
### Use Cases
- GPS - find the fastest route.
- Network Routing - find open shortest path for data.
- Airline Tickets - find the cheapest route to your destination.
- Biology - model the spread of viruses among humans.

## Dijkstra's Algorithm Flow
1. Look to visit a new node. Pick the node with the shortest known distance to visit first (this is what the priority queue is used for).
2. Once we've moved to that node, look at each of its neighbors.
3. For each neighboring node, calculate the distance from the starting node by summing the total edges that lead to that neighboring node.
4. If the new total distance to that neighboring node is less than the previous total, store the new shorter distance for that neighboring node.

## Example
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/dijkstra-flow-1.png" alt="Dijkstra Flow 1" width="45%" />
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/DSA/refImg/dijkstra-flow-2.png" alt="Dijkstra Flow 2" width="45%" />
</p>

- `Previous` represents the current shortest path to any given node from `A`.
- Every time through, pick the current shortest distance from `A` that hasn't been visited. Explore each of its neighbors and calculate the new shortest distance to each neighbor. If, for each neighbor, the newly calculated distance is shorter than what was stored, update what was stored and reflect the change on the previous node tracker.
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
### Priority Queue
- Purpose:
  - To give us the next node in line to visit.
    - `dequeue` the vertex with the shortest distance from `A` currently.
  - In effect, acts as a tracker for vertices that have already been visited.
    - If a vertex hasn't been visited (i.e. its distance from `A` hasn't been calculated yet), there is no point in checking its neighbors.
- [Implementation here](https://github.com/Kakamotobi/Learned/blob/main/DSA/14-Priority-Queue.md#priority-queue-implementation-using-min-binary-heap)

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
    - If the distance is less than what is currently stored in `distancesFromStart`.
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
    const verticesPQ = new PriorityQueue();
    const distancesFromStart = {};
    const previousVertices = {};
    let path = [];
    
    // Build up initial state for distancesFromStart, verticesPQ and previousVertices.
    for (let vertex in this.adjacencyList) {
      if (vertex === start) {
        distancesFromStart[vertex] = 0;
        verticesPQ.enqueue(vertex, 0);
      } else {
        distancesFromStart[vertex] = Infinity;
        verticesPQ.enqueue(vertex, Infinity);
      }
      previousVertices[vertex] = null;
    }
    
    let shortestVertexVal;
    // While there is something in verticesPQ.
    while (verticesPQ.nodes.length) {
      shortestVertexVal = verticesPQ.dequeue().val;
      
      // If reached the destination.
      if (shortestVertexVal === finish) {
        // Build up the path.
        while (previousVertices[shortestVertexVal]) {
          path.push(shortestVertexVal);
          shortestVertexVal = previousVertices[shortestVertexVal];
        }
        break;
      }
      
      if (shortestVertexVal || distancesFromStart[shortestVertexVal] !== Infinity) {
        for (let neighbor in this.adjacencyList[shortestVertexVal]) {
          // Find neighboring vertex.
          const neighborVertex = this.adjacencyList[shortestVertexVal][neighbor];
          // Calculate new distance to neighbor vertex (current distance for our vertex + distance to the neighbor).
          const distanceToNeighborVertex = distancesFromStart[shortestVertexVal] + neighborVertex.weight;
          // If the newly calculated distance is shorter than what is currently stored.
          if (distanceToNeighborVertex < distancesFromStart[neighborVertex.vertex]) {
            // Update distance to neighbor.
            distancesFromStart[neighborVertex.vertex] = distanceToNeighborVertex;
            // Enqueue the neighbor in priority queue with new priority.
            verticesPQ.enqueue(neighborVertex.vertex, distanceToNeighborVertex);
            // Update how we got to neighbor.
            previousVertices[neighborVertex.vertex] = shortestVertexVal;
          }
        }
      }
    }
    
    return path.concat(shortestVertexVal).reverse();
  }
}
```

## Reference
[Greedy Algorithms | Brilliant Math & Science Wiki](https://brilliant.org/wiki/greedy-algorithm/)  
[shortest path - Why doesn't Dijkstra's algorithm work for negative weight edges? - Stack Overflow](https://stackoverflow.com/questions/13159337/why-doesnt-dijkstras-algorithm-work-for-negative-weight-edges#:~:text=Since%20Dijkstra's%20goal%20is%20to,nodes%20that%20it%20has%20visited.)
