# Dijkstra's Algorithm

## Table of Contents
- [What is Dijkstra's Algorithm?](#what-is-dijkstras-algorithm)
- [Dijkstra's Algorithm Flow - Greedy](#dijkstras-algorithm-flow---greedy)
- [Essential Data Structures for Dijkstra's Algorithm](#essential-data-structures-for-dijkstras-algorithm)
  - [Weighted Graph](#weighted-graph)
  - [Priority Queue](#priority-queue)
- [Dijkstra's Algorithm Implementation](#dijkstras-algorithm-implementation)
- [Reference](#reference)

## What is Dijkstra's Algorithm?
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

## Dijkstra's Algorithm Flow - Greedy
1. Look to visit a new node. **Pick the node with the shortest known distance to visit first (this is what the priority queue is used for)**.
2. Once we've moved to that node, look at each of its neighbors.
3. **For each neighboring node, calculate the distance from the starting node** by summing the total edges that lead to that neighboring node.
4. If the calculated distance to that neighboring node is less than the previously calculated distance, update the shortest path to that node by **storing the new shorter distance for that neighboring node**.
### Example
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
    - The vertex's priority represents the distance from `A`.
  - In effect, acts as a tracker for vertices that have already been visited.
    - If a vertex hasn't been visited (i.e. its distance from `A` hasn't been calculated yet), there is no point in checking its neighbors.
- Why use a Priority Queue instead of a normal Queue?
  - Because for every iteration, we want to pick the vertex with the shortest distance of all available vertices in O(1) time complexity.
  - If not using a Priority Queue (that is implemented with a binary heap), it can take a lot of time to determine the vertex to pick.
- [Implementation here](https://github.com/Kakamotobi/Learned/blob/main/DSA/14-Priority-Queue.md#priority-queue-implementation-using-min-binary-heap)

## Dijkstra's Algorithm Implementation
1. Define the algorithm as a function in the Weighted Graph Class.
    - The function should accept a starting and ending vertex (Ex: `A` to `E`).
2. Initialize a few data structures.
    - Priority Queue where **each vertex has a priority that represents the distance from the starting vertex**.
      - It serves to determine the next vertex to visit.
      - Initialize all vertices' priorities to `Infinity` except for the starting vertex with `0`.
      - Example
        ```js
        [
          { vertex: A, priority: 0 },
          { vertex: B, priority: Infinity },
          { vertex: C, priority: Infinity },
          // ...
        ]
        ```
    - A Hash Map called `shortestDistsFromStart` storing the **current shortest distance from the starting vertex to each vertex**.
      - i.e. the currently calculated minimum distance that each vertex is from the starting vertex.
      - Initialize each vertex's shortest distance to `Infinity` (since they are yet to be calculated) except for the starting vertex initialized to `0`.
      - Example
        ```js
        {
          A: 0,
          B: Infinity,
          C: Infinity,
          // ...
        }
        ```
      - The objective is to "greedily" calculate/update the shortest distances from the starting vertex to each vertex.    
    - Hash Map called `previousVertices` storing the **previous node to this node on the shortest path from the starting vertex**.
      - i.e. what node did we come from on the shortest path to get to this node?
      - Initialize all values to `null`.
      - Example
        ```js
        {
          A: null,
          B: null,
          C: null,
          // ...
        }
        ```
      - This should update every time any value in `shortestDistsFromStart` updates.
      - *`previousVertices` is only used at the end of the algorithm, when it ends up representing the shortest path from the starting vertex to any given vertex that lies between the ending vertex.*
3. For as long as the Priority Queue is not empty,
    - Pick the vertex with the shortest distance from A currently that has not been visited already.
      - i.e. dequeue (root) vertex from the Priority Queue.
    - If that vertex is the ending vertex, compute the path, and end algorithm.
    - Otherwise, loop through each of the vertex's neighbors (using the `adjacencyList`).
      - Calculate the shortest distance from the starting vertex to the neighbor.
        - i.e. retrieve the current vertex's shortest distance from `shortestDistsFromStart`. Then, add that value with the distance to the neighbor from the current vertex (retrieved from the `adjacencyList`).
      - If the newly calculated distance is less than what is currently stored in `shortestDistsFromStart`, 
        - Update the corresponding values in `shortestDistsFromStart` and `previousVertices`.
        - Enqueue the neighbor with the new shortest distance form the starting vertex to the Priority Queue.
    - Record the vertex as visited.
### Code Example
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
    const shortestDistsFromStart = {};
    const previousVertices = {};
    const finalPath = [];
    
    // Build up initial state of verticesPQ, shortestDistsFromStart and previousVertices.
    for (let vertex in this.adjacencyList) {
      if (vertex === start) {
        verticesPQ.enqueue(vertex, 0);
        shortestDistsFromStart[vertex] = 0;
      } else {
        verticesPQ.enqueue(vertex, Infinity);
        shortestDistsFromStart[vertex] = Infinity;
      }
      previousVertices[vertex] = null;
    }
    
    // Calculate the shortest distance for each vertex from the starting vertex.
    let currVertexVal; // pick the vertex with the currently shortest distance from the start.
    // While there is something in `verticesPQ`.
    while (verticesPQ.nodes.length) {
      currVertexVal = verticesPQ.dequeue().val; // Ex: "A"
      
      // If reached the destination, end the algorithm.
      if (currVertexVal === finish) break;
      
      // If the current vertex has already been visited (a shortest distance has been calculated for it), proceed (look at its neighbors).
      // The current vertex has to have been visited for the algorithm to be "greedy".
      if (currVertexVal && shortestDistsFromStart[currVertexVal] !== Infinity) {
        // Loop through each neighbor (Ex: `{ vertex: "B", weight: 4 }`).
        for (let { vertex: neighborVertexVal, weight } of this.adjacencyList[currVertexVal]) {
          // Calculate the new distance to the neighbor vertex (distance from start to current vertex + distance from current vertex to neighbor).
          const newDistFromStartToNeighbor = shortestDistsFromStart[currVertexVal] + weight;
          // If the newly calculated distance is shorter than what is currently stored.
            // This condition prevents us from an infinite loop because the newly calculated distance (from the 
            // starting vertex) for an already visited neighbor will always be greater than what's already stored in 
            // `shortestDistsFromStart` since it also includes the distance from the current vertex to the already visited neighbor.
            // Therefore, already visited neighbors will not get added to Priority Queue.
          if (newDistFromStartToNeighbor < shortestDistsFromStart[neighborVertexVal]) {
            // Update shortest distance to neighbor.
            shortestDistsFromStart[neighborVertexVal] = newDistFromStartToNeighbor;
            // Update how we got to neighbor on that shortest path.
            previousVertices[neighborVertexVal] = currVertexVal;
            // Enqueue the neighbor in Priority Queue with new priority.
            verticesPQ.enqueue(neighborVertexVal, newDistFromStartToNeighbor);
          }
        }
      }
    }
    
    // Build up the shortest path from start to finish.
    while (previousVertices[currVertexVal]) {
      finalPath.push(currVertexVal);
      currVertexVal = previousVertices[currVertexVal];
    }
    
    return finalPath.concat(currVertexVal).reverse();
  }
}
```
```js
const graph = new WeightedGraph();

graph.addVertex("A");
graph.addVertex("B");
graph.addVertex("C");
graph.addVertex("D");
graph.addVertex("E");
graph.addVertex("F");

graph.addEdge("A", "B", 4);
graph.addEdge("A", "C", 2);
graph.addEdge("B", "E", 3);
graph.addEdge("C", "D", 2);
graph.addEdge("C", "F", 4);
graph.addEdge("D", "E", 3);
graph.addEdge("D", "F", 1);
graph.addEdge("E", "F", 1);

graph.Dijkstra("A", "E"); // ["A", "C", "D", "F", "E"]
```

## Reference
[Greedy Algorithms | Brilliant Math & Science Wiki](https://brilliant.org/wiki/greedy-algorithm/)  
[shortest path - Why doesn't Dijkstra's algorithm work for negative weight edges? - Stack Overflow](https://stackoverflow.com/questions/13159337/why-doesnt-dijkstras-algorithm-work-for-negative-weight-edges#:~:text=Since%20Dijkstra's%20goal%20is%20to,nodes%20that%20it%20has%20visited.)
