# 2021 KAKAO BLIND RECRUITMENT

## [신규 아이디 추천](https://programmers.co.kr/learn/courses/30/lessons/72410)
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
