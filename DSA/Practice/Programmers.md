# Programmers

## [카카오 인턴] 키패드 누르기
- Input: An array of numbers between 0 to 9. A preferred hand ("left" or "right").
- Output: A string of sequences of which hand/thumb was used for pressing each number.
- Rules
  - Use the left thumb for numbers 1, 4, 7.
  - Use the right thumb for numbers 3, 6, 9.
  - Use the nearest thumb for numbers 2, 5, 8, 0.
    - If both are equally near, use the thumb on the preferred hand.
### Expectation
```js
solution([1,3,4,5,8,2,1,4,5,9,5]), "right") // "LRLLLRLLRRL"
solution([7,0,8,2,8,3,1,5,7,6,2], "left") // "LRLLRRLLLRR"
```
### Approach
```js
// Recreate numpad as subarrays (effectively creating a coordinate plane).

// Initialize thumb locations to keep track of.

// Initialize result string.

// Loop over numbers.
  // If num is 1, 4, or 7, use left thumb.
  // If num is 3, 6, or 9, use rigth thumb.
  // If num is 2, 5, 8, or 0.
    // Calculate the distance from each thumb to num.
    // If left thumb is closer, use left thumb.
    // If right thumb is closer, use right humb.
    // If both are equally close, use thumb of preferred hand.

// Return result.
```
### Solution
```js
function solution(numbers, hand) {
  // Initialize numpad.
  const numpad = [
      [1,2,3],
      [4,5,6],
      [7,8,9],
      ["*",0,"#"]
  ];

  // Initialize thumb locations.
  let leftThumb = "*";
  let rightThumb = "#";

  // Initialize result string.
  let result = "";

  for (let num of numbers) {
    if (num === 1 || num === 4 || num === 7) {
      // "Move" left thumb to num.
      leftThumb = num;
      result += "L";
    }
    else if (num === 3 || num === 6 || num === 9) {
      // "Move" right thumb to num.
      rightThumb = num;
      result += "R";
    }
    // if num is 2, 5, 8, or 0.
    else {
      // Find the distances for each thumb to the target num.
      const leftThumbToNum = calculateDistanceToNum(numpad, leftThumb, num);
      const rightThumbToNum = calculateDistanceToNum(numpad, rightThumb, num);

      // If left thumb is closer.
      if (leftThumbToNum < rightThumbToNum) {
        leftThumb = num;
        result += "L";
      }
      // Else if right thumb is closer.
      else if (rightThumbToNum < leftThumbToNum) {
        rightThumb = num;
        result+= "R";
      } 
      // Else they are equally close.
      else {
        // Use thumb on preferred hand.
        if (hand === "left") {
          leftThumb = num;
          result += "L";
        } else if (hand === "right") {
          rightThumb = num;
          result += "R";
        }
      }
    }
  }

  return result;
}

function calculateDistanceToNum(numpad, thumb, num) {
  const thumbCoords = getCoordinates(numpad, thumb);
  const numCoords = getCoordinates(numpad, num);

  // |(x1-x2)| + |(y1-y2)| distance.
  return Math.abs(thumbCoords[0] - numCoords[0]) + Math.abs(thumbCoords[1] - numCoords[1]);
}

function getCoordinates(numpad, target) {
  let coords = [];
  // Loop over each "row" (i represents y-axis).
  for (let i = 0; i < numpad.length ; i++) {
    // Loop over each "column" (j represents x-axis).
    for (let j = 0; j < 3; j++) {
      if (numpad[i][j] === target) coords.push(j, i);
    }
  }
  return coords;
}
```