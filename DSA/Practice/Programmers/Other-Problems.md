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

## Rank Test Takers
- **Input:** an array of arrays `scores` representing the scores of test takers.
- **Description**
  - There are two problems that were given in the test.
  - Ranking System:
    1) The person with the highest total score comes first. 
        - If the total score is the same, apply b.
    2) The person with the higher score on the difficult problem comes first.
        - The difficult problem is the one with the lower sum of scores of all test takers.
       - If the scores are the same on the difficult problem, OR if the more difficult problem doesn't exist (i.e. both problems share the same sum of scores), apply c.
    3) The person with the lower ID number comes first.
- **Output:** return the rank of each test taker in order, in an array, from the first test taker to the last.
- **Constraints**
  - `1 <= scores.length <= 100,000`.
  - `scores[i] = [a,b]`
    - `a` is a positive integer from 0 to 100 representing the test taker's score for the first problem.
    - `b` is a positive integer from 0 to 100 representing the test taker's scoer for the second problem.
### Example
```js
solution[[85, 90], [65, 67], [88, 87], [88, 87], [73, 81], [65, 89],[99, 100], [80, 94]]); // [4, 8, 2, 3, 6, 7, 1, 5]
solution([[85, 90], [91, 87], [88, 87]]);	// [2, 1, 3]
```
### Solution
- In a hashtable, accumulate each test taker's scores for each problem and total score. Also figure out the total scores of all test takers for each problem.
- Sort the test takers by the given rank system.
  - This represents the ids of each test taker in order of ranking ((index + 1)<sup>th</sup> place).
  - Ex: `ids[0] = 7` means that the test taker with id 7 came in first place.
- Using the above array, reorganize so that the indices represent ids in order and their values are their respective ranking.
  - Ex: `ans[0] = 4` means that the test taker with id 1 came in fourth place.
```js
const solution = (scores) => {
  const testTakers = {};
  let problem1Total = 0;
  let problem2Total = 0;
  
  for (let i = 0; i < scores.length; i++) {
    const [p1Score, p2Score] = scores[i];
    testTakers[i+1] = { p1Score, p2Score, totalScore: p1Score + p2Score };
    problem1Total += p1Score;
    problem2Total += p2Score;
  }
  
  const ids = Object.keys(testTakers);
  ids.sort((a,b) => {
    if (testTakers[a].totalScore === testTakers[b].totalScore) {
      if (problem1Total < problem2Total) {
        if (testTakers[a].p1Score === testTakers[b].p1Score) {
          return a - b;
        }
        return testTakers[b].p1Score - testTakers[a].p1Score;
      } else if (problem1Total > problem2Total) {
        if (testTakers[a].p2Score === testTakers[b].p2Score) {
          return a - b;
        }
        return testTakers[b].p2Score - testTakers[b].p2Score;
      } else {
        return a - b;
      }
    } else {
      return testTakers[b].totalScore - testTakers[a].totalScore;
    }
  });
  
  const ans = [];
  for (let i = 0; i < ids.length; i++) {
    ans[parseInt(ids[i]) - 1] = i + 1;
  }
  return ans;
}
```
