# Level 0 - 01

## [옹알이 (1)](https://school.programmers.co.kr/learn/courses/30/lessons/120956)
### Solution
- Remove all occurrences of `"aya"`, `"ye"`, `"woo"`, `"ma"` in all strings in `babbling`.
- Strings that end up empty (`""`) are pronouncable words.
```js
// TC: O(m*n), SC: O(n)
  // m - babbling[i].length
  // n - babbling.length

const solution = (babbling) => {
  const parsedWords = babbling.map(word => word.replace(/aya|ye|woo|ma/g, ""));
  const filteredWords = parsedWords.filter(word => word === "");
  return filteredWords.length;
}
```

## [안전지대](https://school.programmers.co.kr/learn/courses/30/lessons/120866)
### Solution
- When encountered a `1`, collect all the surrounding danger zones (including itself).
- Use a Set to handle overlapping danger zones for adjacent `1`s.
```js
// TC:O(m*n), SC: O(m*n)
  // m - board[i].length
  // n - board.length

function solution(board) {
  const dangerZones = new Set(); // Use a Set to handle overlapping danger zones.

  const directions = [[-1,-1],[-1,0],[-1,1],[0,1],[1,1],[1,0],[1,-1],[0,-1]];
  let newI;
  let newJ;
  for (let i = 0; i < board.length; i++) {
    for (let j = 0; j < board[0].length; j++) {
      if (board[i][j] === 1) {
        dangerZones.add(`${i},${j}`);
        for (let [addI, addJ] of directions) {
          newI = i + addI;
          newJ = j + addJ;
          if (newI >= 0 && newI < board.length && newJ >= 0 && newJ < board[0].length) {
            dangerZones.add(`${newI},${newJ}`);
          }
        }   
      }
    }
  }

  return board.length * board[0].length - dangerZones.size;
}
```
