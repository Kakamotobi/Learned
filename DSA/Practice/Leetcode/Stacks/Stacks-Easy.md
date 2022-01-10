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
