# Kakao Questions

## [[2020 카카오 인턴십] 키패드 누르기](https://programmers.co.kr/learn/courses/30/lessons/67256)
- **Input:** an array of numbers between 0 to 9. A preferred hand ("left" or "right").
- **Output:** a string of sequences of which hand/thumb was used for pressing each number.
- **Rules**
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

## [[2021 KAKAO BLIND RECRUITMENT] 신규 아이디 추천](https://programmers.co.kr/learn/courses/30/lessons/72410)
- **Input:** a string `new_id`.
- **Output:** return a new string that represents an id suggestion based on `new_id`.
- **Description**
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

## [[2019 KAKAO BLIND RECRUITMENT] 실패율](https://programmers.co.kr/learn/courses/30/lessons/42889)
- **Inputs**
  - A natural number `N` representing the total number of stages.
  - An array of natural numbers `stages` where `stages[i]` represents the current stage that a user is currently at.
- **Output:** return an array consisting of the stage numbers in decreasing order of failure rate (stage with highest failure rate to stage with lowest failure rate).
  - Failure Rate = Number of users that have arrived at the stage but haven't cleared the stage yet / Number of users that have arrived (those that have passed or are at) at the stage.
- **Constraints**
  - `1 <= N <= 500`.
  - `1 <= stages.length <= 200,000`.
  - `1 <= stages[i] <= N + 1`.
    - `N + 1` represents users that have cleare the last (N<sup>th</sup>) stage.
  - If the failure rates are identical, let the earlier stage come first.
  - If no user is at a stage, the failure rate is 0.
### Example
```js
solution(5, [2,1,2,6,2,4,3,3]); // [3,4,2,1,5]
solution(4, [4,4,4,4,4]); // [4,1,2,3]
```
### Solution
- **TC: O(n)**
  - n: the number of elements in `stages`.
- Use a hash table to record the number of users at each stage.
- Calculate the failure rate for each stage.
  - Failure Rate = # of users at the stage / # of users at stages that are greater than or equal to the stage.
  - \# of users at stages that are greater than or equal to the stage = Total # of Users - # of Users at the previous stage.
    - Ex: # of Users that have attempted (passed & stuck at) Stage 2 = Total # of Users - # of Users at Stage 1.
    - If `stages.length` is `8`, all `8` users have been at Stage 1. Therefore, the number of users at Stage 2 would be `8 - the number of users that are still at Stage 1`.
- Sort the stages by failure rate in decreasing order.
```js
const solution = (N, stages) => {
  const obj = {};
  
  // Record the number of users at each stage.
  for (let stage of stages) {
    if (stages <= N) {
      obj[stage] = ++obj[stage] || 1;
    }
  }
  
  let numAttemptedUsers = stages.length;
  // Calculate the failure rates of each stage and replace values in obj.
  for (let i = 1; i <= N; i++) {
    let temp = obj[i];
    obj[i] = obj[i] / numAttemptedUsers || 0; // Failure rate should be 0 if there are no users at the stage.
    if (temp) numAttemptedUsers -= temp;
  }
  
  // Collect the map keys in an array.
  const keys = Object.keys(obj);
  // Sort the keys by their values in decreasing order.
  keys.sort((a,b) => obj[b] - obj[a]);
  
  // Convert the keys back to Numbers.
  for (let i = 0; i < keys.length; i++) {
    keys[i] = parseInt(keys[i]);
  }
  
  return keys;
}
```
#### Same Solution Using `Map`
- The above solution can be implemented using JavaScript's `Map`. However, `Map` stores data in insertion order of the keys.
- So, if `stages = [2,1,2,6,2,4,3,3]`, the hash table will be `{2 => 3, 1 => 1, 4 => 1, 3 => 2}` for the number of users at each stage.
  - `2` is the first in the hash table because it is the first number that was inserted.
  - *cf. Using JavaScript Object: `{1: 1, 2: 3, 3: 2, 4: 1}`.*
- If we continue through the algorithm, the hash table becomes `{2 => 0.42857142857142855, 1 => 0.125, 4 => 0.5, 3 => 0.5, 5 => 0}` for the failure rates of each stage.
  - *cf. Using JavaScript Object: `{1: 0.125, 2: 0.42857142857142855, 3: 0.5, 4: 0.5, 5: 0}`.*
- Therefore, the final `keys` sorted in decreasing order of failure rates will be `[4,3,2,1,5]` since Stage 4 was inserted earlier than Stage 3.
  - *cf. Using JavaScript Object: `[3,4,2,1,5]`.*
- However, one of the constraints mentioned that if two stages have the same failure rates, the earlier stage (Stage 3 in this case) should come before. Using `Map`, as it inserts in insertion order, does not guarantee this.
- To solve this, we could sort `stages` in increasing order at the beginning of the algorithm. However, this results to a slower time complexity (O(n log n)).

## [[2021 카카오 채용연계형 인턴십] 숫자 문자열과 영단어](https://programmers.co.kr/learn/courses/30/lessons/81301)
- **Input:** a string of numbers `s` where some numbers are written in plain English.
- **Output:** return the number that the string is supposed to represent.
- **Constraints**
  - `1 <= s.length <= 50`.
  - `s` never starts with a `"0"` or `"zero"`.
  - `s` is given so that `1 <= return value <= 2,000,000`.
### Example
```js
solution("one4seveneight"); // 1478
solution("23four5six7"); // 234567
```
### Solutions
#### Solution 1 - O(n)
- Use built-in String and Array methods to pick out and replace numbers in alphabet form to digits.
```js
const solution = (s) => {
  let nums = ["zero","one","two","three","four","five","six","seven","eight","nine"];
  
  for (let i = 0; i < nums.length; i++) {
    s = s.split(nums[i]).join(i); // Locate each number from 0 - 9 in their English forms, replace them with the number in digits.
  }
  
  return parseInt(s);
}
```
Or
```js
// ES2021+

const solution = (s) => {
  let nums = ["zero","one","two","three","four","five","six","seven","eight","nine"];
  
  for (let i = 0; i < nums.length; i++) {
    s = s.replaceAll(nums[i], i); // Replace English forms intwo digits.
  }
  
  return parseInt(s);
}
```
#### Solution 2
- Use Regex.
```js
function solution(s) {
  s = s.replace(/zero/g, 0)
    .replace(/one/g, 1)
    .replace(/two/g, 2)
    .replace(/three/g, 3)
    .replace(/four/g, 4)
    .replace(/five/g, 5)
    .replace(/six/g, 6)
    .replace(/seven/g, 7)
    .replace(/eight/g, 8)
    .replace(/nine/g, 9);
    
  return parseInt(s);
}
```

## [[2019 카카오 개발자 겨울 인턴십]크레인 인형뽑기 게임](https://programmers.co.kr/learn/courses/30/lessons/64061)
- **Inputs**
  - A 2D array `board` representing a square board.
    - `board[i]` represents a row of the game board (starting from the most top row).
    - `board[i][j]` represents a doll in that slot.
  - An array of positive integers `moves` representing the "column" of the board in which the crane picked a doll.
- **Description**
  - Every time the crane picks a doll, the doll is removed from the board and stacked in a separate pile.
  - If the same two dolls are stacked consecutively, they both "pop" from the pile.
- **Output:** return the number of dolls that have been "popped" from the pile.
- **Constraints**
  - `0 <= board[i][j] <= 100`.
    - A doll is represented as a a number from 1 to 100.
    - 0 means that there is no doll in that slot.
  - `1 <= moves.length <= 1000`.
  - `1 <= moves[i] <= board.length`.
    - `moves[i]` is guaranteed to be a valid column in the board.
### Example
```js
solution([[0,0,0,0,0],[0,0,1,0,3],[0,2,5,0,1],[4,2,4,4,2],[3,5,1,3,1]], [1,5,3,5,1,2,1,4]); // 4

  // Board Before              Board After
  // [[0,0,0,0,0],             [[0,0,0,0,0],
  //  [0,0,1,0,3],              [0,0,0,0,0],
  //  [0,2,5,0,1],      =>      [0,0,5,0,0],
  //  [4,2,4,4,2],              [0,2,4,0,2],
  //  [3,5,1,3,1]]              [0,5,1,3,1]]
```
### Solution
- **TC: O(m\*b)**
  - m: the number of moves made by the crane (`moves.length`).
  - b: the number of rows in board (i.e. `board.length` since it is a square board).
```js
const solution = (board, moves) => {
  const basket = [];
  let count = 0;
  
  for (let move of moves) {
    for (let row of board) {
      if (row[move-1] > 0) {
        if (row[move-1] === basket[basket.length-1]) {
          basket.pop();
          count += 2;
        } else {
          basket.push(row[move-1]);
        }
        row[move-1] = 0;
        break;
      }
    }
  }
  
  return count;
}
```

## [[2022 KAKAO BLIND RECRUITMENT] 신고 결과 받기](https://programmers.co.kr/learn/courses/30/lessons/92334)
- **Inputs**
  - An array of strings `id_list`, representing a list of user IDs.
  - An array of strings `report`, representing which user reported which user.
    - For each string, `"reporter reported"`.
  - A positive integer `k`, representing the number of reports required for a user to be blocked.
- **Output:** return an array consisting of the number of emails that each user has received (in the same order as `id_list`).
- **Rules**
  - Each user can report a single user per report.
    - There is no limit to the number of reports that can be made.
    - Multiple reports on the same user can be made by the same user. But it will only count as 1.
  - A user that has been reported `k` times is blocked. And every user that has reported that user should receive an email noticing this.
### Example
```js
solution(["muzi", "frodo", "apeach", "neo"], ["muzi frodo","apeach frodo","frodo neo","muzi neo","apeach muzi"], 2); // [2,1,1,0]
solution(["con", "ryan"], ["ryan con", "ryan con", "ryan con", "ryan con"], 3); // [0,0]
```
### Solution
- Construct a hashtable where the key is those that have been reported and the value is a Set of the reporters.
- Use the above hashtable to construct another hashtable representing the number of emails that a reporter should receive.
- Use the above hashtable and `id_list` to return the number of emails each user should receive.
```js
const solution = (id_list, report, k) => {
  const whoReportedMe = {};
  const numEmails = {};
  
  // Construct whoReportedMe.
  for (let r of report) {
    const users = r.split(" ");
    
    whoReportedMe[users[1]] = whoReportedMe[users[1]] !== undefined ? whoReportedMe[users[1]].add(users[0]) : new Set([users[0]]);
    // or ES2020+
    // whoReportedMe[users[1]] = whoReportedMe[users[1]]?.add(users[0]) || new Set([users[0]]);
  }
  
  // Construct numEmails.
  for (let key in whoReportedMe) {
    if (whoReportedMe[key].size >= k) {
      for (let reporter of whoReportedMe[key].values()) {
        numEmails[reporter] = ++numEmails[reporter] || 1;
      }
    }
  }
  
  // Compute results.
  for (let i = 0; i < id_list.length; i++) {
    id_list[i] = id_list[i] in numEmails ? numEmails[id_list[i]] : 0;
  }
  
  return id_list;
}
```



