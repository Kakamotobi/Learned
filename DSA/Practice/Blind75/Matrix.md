# Matrix

## 1. [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)
### Solution 1
- Loop through each element in the matrix.
- If encountered a `0` that was not a result of a conversion, convert the whole row and column to `0`s.
```js
// TC: O(n * m), SC: O(n)
  // n - number of elements in the matrix.
  // m - the length of the row + column of an element.

const setZeroes = (matrix) => {
  const alreadyConverted = {};
  
  for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[i].length; j++) {
      // If number is 0 and it was not already converted.
      if (matrix[i][j] === 0 && !alreadyConverted[i]?.has(j)) {
        // Convert whole row to 0s.
        for (let k = 0; k < matrix[i].length; k++) {
          // Record only non 0s as converted.
          if (matrix[i][k] !== 0) {
            alreadyConverted[i] = alreadyConverted[i] ? alreadyConverted[i].add(k) : new Set([k]);
          }
          // Convert to 0.
          matrix[i][k] = 0;
        }
        
        // Convert whole column to 0s.
        for (let k = 0; k < matrix.length; k++) {
          if (matrix[k][j] !== 0) {
            alreadyConverted[k] = alreadyConverted[k] ? alreadyConverted[k].add(j) : new Set([j]);
          }
          matrix[k][j] = 0;
        }
      }
    }
  }
  
  return matrix;
}
```
### Solution 2
- Loop through each element in the matrix.
  - If encountered a `0`, convert the leftmost element to `0` to indicate that the row should be converted to `0`, and the topmost element to indicate that the column should be converted to `0`.
- Only need a single boolean variable to indicate whether the first row should be converted to `0`s or not. The rest can be done in place.
```js
// TC: O(n * m), SC: O(1)

const setZeroes = (matrix) => {
  let firstRow = false;
  
  // Determine which rows and columns should be converted to zero.
  for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[0].length; j++) {
      if (matrix[i][j] === 0) {
        // Mark the topmost element to mark this column.
        matrix[0][j] = 0;
        // Mark the leftmost element (or variable if first row) to mark this row.
        if (i > 0) matrix[i][0] = 0;
        else firstRow = true;
      }
    }
  }
  
  // Convert the main part to `0`.
  for (let i = 1; i < matrix.length; i++) {
    for (let j = 1; j < matrix[0].length; j++) {
      if (matrix[0][j] === 0 || matrix[i][0] === 0) matrix[i][j] = 0;
    }
  }
  
  // Convert first column to `0` if needed.
  if (matrix[0][0] === 0) {
    for (i = 0; i < matrix.length; i++) {
      matrix[i][0] = 0;
    }
  }
  
  // Convert first row to `0` if needed.
  if (firstRow) {
    for (let j = 0; j < matrix[0].length; j++) {
      matrix[0][j] = 0;
    }
  }
  
  return matrix;
}
```

## 2. [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
- Keep clearing the outer layers, effectively shrinking the matrix.
- Use boundaries (top, bottom, left, right) to keep track of until what point has been recorded.
  - Shrink the boundary as the layer for that boundary is cleared.
  - Stop if any boundaries are no longer valid.
```js
// TC: O(n * m), SC: O(1)

const spiralOrder = (matrix) => {
  const ans = [];
  
  let left = 0;
  let right = matrix[0].length;
  let top = 0;
  let bottom = matrix.length;
  
  while (left < right && top < bottom) {
    // Get all elements in the top row.
    for (let i = left; i < right; i++) ans.push(matrix[top][i]);
    top++;
    // Get all elements in the right column.
    for (let i = top; i < bottom; i++) ans.push(matrix[i][right - 1]);
    right--;
    
    // Edge Case: if matrix is a single row or column.
    if (!(left < right && top < bottom)) break;
    
    // Get all elements in the bottom row.
    for (let i = right - 1; i >= left; i--) ans.push(matrix[bottom - 1][i]);
    bottom--;
    // Get all elements in the left column.
    for (let i = bottom - 1; i >= top; i--) ans.push(matrix[i][left]);
    left++;
  }
  
  return ans;
}
```

## 3. [Rotate Image](https://leetcode.com/problems/rotate-image/)
- Handle rotation layer by layer (i.e. rotate outer layer first).
  - Use boundary pointers (left, right, top, bottom) to keep track of which layer is being worked on.
- For each layer, rotate the element in the same positions first (Ex: first element on each top, right bottom, left). Then, rotate the second, third, etc. on each side.
- To minimize temporary variables to one, store the top-left element only and start rotation with the bottom-left (to top-left). Order is counter-clockwise.
```js
// TC: O(n^2), SC: O(1)

const rotate = (matrix) => {
  let left = 0;
  let right = matrix.length - 1;
  let top = 0;
  let bottom = matrix.length - 1;
  
  while (left < right) {
    // Make rotations for right - left elements (elements in this layer).
    // Use `i` to offset the current position that is being rotated.
    for (let i = 0; i < right - left; i++) {
      // Temporarily store the top-left element.
      const tempTopLeft = matrix[top][left + i];
      
      // Rotate the bottom-left to top-left.
      matrix[top][left + i] = matrix[bottom - i][left];
      
      // Rotate the bottom-right to bottom-left.
      matrix[bottom - i][left] = matrix[bottom][right - i];
      
      // Rotate the top-right to bottom-right.
      matrix[bottom][right - i] = matrix[top + i][right];
      
      // Rotate the top-left to top-right.
      matrix[top + i][right] = tempTopLeft;
    }
    
    // Update boundaries (move on to inner layer).
    left++;
    right--;
    top++;
    bottom--;
  }
}
```
