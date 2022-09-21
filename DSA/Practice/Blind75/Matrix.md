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
