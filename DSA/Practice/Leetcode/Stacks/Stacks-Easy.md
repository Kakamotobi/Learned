# Stacks - Easy

## 1614. Maximum Nesting Depth of the Parentheses
- Input: a valid parentheses string.
- Output: the maximum number of nested parentheses.
### Example
```js
// Input: "(1+(2*3)+((8)/4))+1"
// Output: 3
```
### Solution 1
- TC: O(n)
- SC: O(1)
```js
const maxDepth = (s) => {
  let maxCount = 0;
  let count = 0;
  for (let char of s) {
    if (char === "(") maxCount = Math.max(maxCount, ++count);
    else if (char === ")") count--;
  }
  return maxCount;
}
```
### Solution 2 - Stack
- TC: O(n)
- SC: O(n)
```js
const maxDepth = (s) => {
  let stack = [];
  let maxCount = 0;
  for (let char of s) {
    if (char === "(") {
      stack.push(char);
      maxCount = Math.max(maxCount, stack.length);
    }
    else if (char === ")") stack.pop();
  }
  return maxCount;
}
```

## 844. Backspace String Compare
- Input: two strings `s` and `t`.
- Output: return `true` if they are equal when both are types into empty text editors.
  - `"#"` means a backspace character.
### Example
```js
// Input: s = "ab#c", t = "ad#c"
// Output: true

// Input: s = "ab##", t = "c#d#"
// Output: true
```
### Solution 1 - Stack
- TC: O(n)
- SC: O(n)
```js
const backspaceCompare = (s, t) => {
  return process(s) === process(t);
}

function process(str) {
  let processedStr = [];
  for (let i = 0; i < str.length; i++) {
    if (str[i] !== "#") processedStr.push(str[i]);
    else if (str[i] === "#") processedStr.pop();
  }
  return processedStr.join("");
}
```
### Solution 2 - Two Pointers
- TC: O(n+m)
- SC: O(1)
```js
const backspaceCompare = (s, t) => {
  // Initialize pointers to the end.
  let sIdx = s.length - 1;
  let tIdx = t.length - 1;
  
  while (sIdx >= 0 || tIdx >= 0) {
    // Update pointers to account for backspaces.
    sIdx = handleSkip(s, sIdx);
    tIdx = handleSkip(t, tIdx);
    // If the characters at each pointer are not the same, return false.
    if (s[sIdx] !== t[tIdx]) return false;
    // Move pointers.
    sIdx--;
    tIdx--;
  }
  
  return true;
}

// Helper to skip the pointer over the number of "#"s.
function handleSkip(str, idx) {
  // Initialize num of characters to skip/backspace.
  let skip = 0;
  
  while (idx >= 0) {
    // If char is "#", increment skip.
    if (str[idx] === "#") skip++;
    // Else if char is not "#" and there remain characters to skip.
    // i.e., encountered a char that needs to be backspaced.
    else if (str[idx] !== "#" && skip > 0) skip--;
    // Else, break the loop.
    else break;
    // Move on to next char.
    idx--;
  }
  
  return idx;
}
```
