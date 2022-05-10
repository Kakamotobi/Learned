# Other Problems

## Remove Row/Column Blocks
- **Input**
  - An array of strings `board` representing an N x N square grid consisting of blocks with various colors.
    - A block is a connection of 1 x 1 block(s).
    - Blocks form together to exactly fill the grid.
    - The different colors are represented as different alphabets.
- **Description**
  - Select either a single row or column, and delete the whole row/column.
    - *All connected blocks have to be deleted as well.*
      - Ex: if a blue block on the row/column extends its area outside of the row/column, delete those as well.
- **Output:** return the maximum possible number of blocks that can be deleted from deleting a single row/column.
- **Constraints**
  - `3 <= board.length <= 50`.
  - `board[i] === board.length`.
  - `board[i]` represents a row on the board.
  - `board[i][j]` is a capitalized alphabet.
    - The same alphabet refers to the same color.
    - Alphabets that are connected left, right, top, bottom form a block.
### Example
```js
solution(["ABBBBC","AABAAC","BCDDAC","DCCDDE","DCCEDE","DDEEEB"]); // 20
solution(["DDCCC","DBBBC","DBABC","DBBBC","DDCCC"]); // 25
```
### Solution
- Calculate the number of blocks that can be deleted for all rows and columns.
- When looping through each row/column, execute breadth-first search for each block that has not been visited (i.e., coordinates haven't been visited).
```js
const solution = (board) => {
  const moves = [[-1,0],[0,1],[1,0],[0,-1]];
  
  // BFS to calculate the number of blocks that are connected to the block along the row or column.
  const bfs = (x, y) => {
    let numBlocksInRegion = 0;
    const currRegion = board[x][y];
    const visited = {};
    const queue = [[x,y]];
    while (queue.length > 0) {
      const currBlock = queue.shift();
      const [coordX, coordY] = currBlock;
      
      // If coord hasn't been visited yet.
      if (!visited[coordX]?.includes(coordY)) {
        numBlocksInRegion++;
      }
      
      // Record coord as visited.
      if (visited[coordX]) visited[coordX].push(coordY);
      else visited[coordX] = [coordY];
      
      // Check neighbors.
      for (let move of moves) {
        const newX = coordX + move[0];
        const newY = coordY + move[1];
        
        // If neighbor block is valid AND is the same region && hasn't been visited.
        if (newX >= 0 && newX < board.length && newY >= 0 && newY < board.length
            && board[newX][newY] === currRegion
            && !visited[newX]?.includes(newY)) {
          queue.push([newX, newY]);    
        }
      }
    }
    return numBlocksInRegion
  }
  
  // Rows
  let mostNumBlocksRows = 0;
  for (let i = 0; i < board.length; i++) {
    let totalNumBlocksForThisRow = 0;
    const visitedRegion = new Set();
    for (let j = 0; j < board.length; j++) {
      if (!visitedRegion.has(board[i][j]) {
        totalNumBlocksForThisRow += bfs(i, j);
        visitedRegion.add(board[i][j]);
      }
    }
    mostNumBlocksRows = Math.max(totalNumBlocksForThisRow, mostNumBlocksRows);
  }
  
  // Columns
  let mostNumBlocksCols = 0;
  for (let i = 0; i < board.length; i++) {
    let totalNumBlocksForThisCol = 0;
    const visitedRegion = new Set();
    for (let j = 0; j < board.length; j++) {
      if (!visitedRegion.has(board[j][i]) {
        totalNumBlocksForThisCol += bfs(j, i);
        visitedRegion.add(board[j][i]);
      }
    }
    mostNumBlocksCols = Math.max(totalNumBlocksForThisCol, mostNumBlocksCols);
  }
  
  return Math.max(mostNumBlocksRows, mostNumBlocksCols);
}
```
