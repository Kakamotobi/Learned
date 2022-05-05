# Flooding Algorithm

## Table of Contents
- [What are Flooding Algorithms?](#what-are-flooding-algorithms)
- [Flood Fill Algorithm](#flood-fill-algorithm)

## What are Flooding Algorithms?
> A flooding algorithm is an algorithm for distributing material to every part of a graph. | Wikipedia

> Flooding algorithms are also useful for solving many mathematical problems, including maze problems and many problems in graph theory. | Wikipedia

## Flood Fill Algorithm
> Flood fill, also called seed fill, is a flooding algorithm that determines and alters the area connected to a given node in a multi-dimensional array with some matching attribute. | Wikipedia

<p align="center">
  <img src="https://github.com/Kakamotobi/Learned/blob/main/DSA/refImg/flood-fill.gif" alt="Flood Fill Algorithm Illustration" width="80%" />
</p>

- Connected nodes share the same value and have a path between them by moving either left, right, up, or down.

### Use Cases
- Paint bucket fill.
- Minesweeper.
- Go.
### Implementation Using BFS
- `img` is an array of arrays representing a grid of pixel values of an image.
- `row` and `col` represent the the location (indices) of the starting pixel.
- `newPixelVal` represents the value to fill with.
```js
const img = [
  [1,0,2,2,0],
  [0,2,0,2,0],
  [2,2,2,2,2],
  [0,0,2,1,2],
  [2,0,0,0,0]
];

floodFill(img, 2, 2, 5);

[
  [1,0,5,5,0],
  [0,5,0,5,0],
  [5,5,5,5,5],
  [0,0,5,1,5],
  [2,0,0,0,0]
]
```
```js
function floodFill(img, row, col, newPixelVal) {
  const startingPixelVal = img[row][col];
  const queue = [[row, col]]; // Queue for locations of pixels.
  const visited = {};
  
  while (queue.length > 0) {
    const currentPixel = queue.shift();
    const [row, col] = currentPixel;
    
    if (visited[row]) visited[row] = [...visited[row], col];
    else visited[row] = [col];
    
    img[row][col] = newPixelVal;
    
    for (let neighbor of getNeighbors(img, currentPixel, startingPixelVal)) {
      const [row, col] = neighbor;
      if (!visited[row]?.includes(col)) {
        queue.unshift([row, col]);
      }
    }
  }
  
  return img;
}

function getNeighbors(img, pixel, startingPixelVal) {
  const [row, col] = pixel;
  const indices = [[row-1, col], [row+1, col], [row, col-1], [row, col+1]];
  
  const neighbors = [];
  for (let [row, col] of indices) {
    if (isValid(img, row, col) && img[row][col] === startingPixelVal) 
      neighbors.push([row, col]);
  }
  return neighbors;
}

function isValid(img, row, col) {
  return row >= 0 && col >= 0 && row <= img.length && col < img[0].length;
}
```
### Implementation Using DFS
```js
function floodFill(img, row, col, newPixelVal) {
  const startingPixelVal = img[row][col];
  const visited = {};
  
  function dfs(img, row, col, newPixelVal) {
    // Base Case
    if (!row && !col) return;
    
    if (visited[row]) visited[row] = [...visited[row], col];
    else visited[row] = [col];
    
    img[row][col] = newPixelVal;
    
    for (let neighbor of getNeighbors(img, [row, col], startingPixelVal)) {
      const [row, col] = neighbor;
      if (!visited[row]?.includes(col)) {
        dfs(img, row, col, newPixelVal);
      }
    }
  }
  
  dfs(img, row, col, newPixelVal);
  
  return img;
}
```

## Reference
[Flooding Algorithm - Wikipedia](https://en.wikipedia.org/wiki/Flooding_algorithm)  
[Flood fill - Wikipedia](https://en.wikipedia.org/wiki/Flood_fill)  
[Breadth First Search (BFS): Visualized and Explained - YouTube](https://www.youtube.com/watch?v=xlVX7dXLS64&list=PLpXOY-RxVRTPPVLBP6-sz6CMWxhtrI-v_&index=3&ab_channel=Reducible)  
