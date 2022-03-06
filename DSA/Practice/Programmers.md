# Programmers

## [[카카오 인턴] 키패드 누르기](https://programmers.co.kr/learn/courses/30/lessons/67256)
- Input: An array of numbers between 0 to 9. A preferred hand ("left" or "right").
- Output: A string of sequences of which hand/thumb was used for pressing each number.
- Rules
  - Use the left thumb for numbers 1, 4, 7.
  - Use the right thumb for numbers 3, 6, 9.
  - Use the nearest thumb for numbers 2, 5, 8, 0.
    - If both are equally near, use the thumb on the preferred hand.
### Example
```js
solution([1,3,4,5,8,2,1,4,5,9,5]), "right") // "LRLLLRLLRRL"
solution([7,0,8,2,8,3,1,5,7,6,2], "left") // "LRLLRRLLLRR"
```
### Approach
```js
// Recreate numpad as a matrix using subarrays (effectively creating a coordinate plane).
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
const solution= (numbers, hand) => {
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
      const leftThumbToNum = thumbToNumDistance(numpad, leftThumb, num);
      const rightThumbToNum = thumbToNumDistance(numpad, rightThumb, num);

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

function thumbToNumDistance(numpad, thumb, num) {
  const thumbCoords = getCoordinates(numpad, thumb);
  const numCoords = getCoordinates(numpad, num);

  // |(x1-x2)| + |(y1-y2)| distance.
  return Math.abs(thumbCoords[0] - numCoords[0]) + Math.abs(thumbCoords[1] - numCoords[1]);
}

function getCoordinates(numpad, target) {
  // Loop over each "row" (i represents y-axis).
  for (let i = 0; i < numpad.length ; i++) {
    // Loop over each "column" (j represents x-axis).
    for (let j = 0; j < 3; j++) {
      if (numpad[i][j] === target) return [j, i];
    }
  }
}
```

## [[카카오 Blind Recruitment] 신규 아이디 추천](https://programmers.co.kr/learn/courses/30/lessons/72410)
- Input: a string `new_id`.
- Output: return a new string that represents an id suggestion based on `new_id`.
- Description
  - Step 1: Convert all alphabets to lowercase.
  - Step 2: Exclude all characters except for lowercase alphabets, digits, `-`, `_`, `.`.
  - Step 3: Replace consecutive `.`s with a single `.`.
  - Step 4: Remove `.` from the beginning and end if it exists.
  - Step 5: If string is empty, set string to be `a`.
  - Step 6-1: If the string length is greater than 15, exclude all characters after the 15th character.
  - Step 6-2: If after 6-1 a `.` is at the end of the string, remove the `.`.
  - Step 7: If the string length is less than 3, fill the string with the last character until it has a length of 3.
### Example
```js
solution("...!@BaT#*..y.abcdefghijklm"); // "bat.y.abcdefghi"
solution("z-+.^."); // "z--"
solution("=.="); // "aaa"
```
### Solution
- Use regular expressions for the most part.
```js
const solution = (new_id) => {
  // Step 1
  new_id = new_id.toLowerCase();
  // Step 2
  new_id = new_id.replace(/[^a-z0-9\-\_\.]/g, "");
  // Step 3
  new_id = new_id.replace(/\.{2,}/g, ".");
  // Step 4
  new_id = new_id.replace(/^\.|\.$/g, "");
  // Step 5
  if (new_id === "") new_id = "a";
  // Step 6-1
  if (new_id.length > 15) new_id = new_id.substring(0, 15);
  // Step 6-2
  new_id = new_id.replace(/\.$/g, "");
  // Step 7
  if (new_id.length < 3) new_id = new_id.padEnd(3, new_id[new_id.length-1]);
  
  return new_id;
}
```
