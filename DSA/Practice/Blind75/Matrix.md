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
