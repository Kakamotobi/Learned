# Strings

## 1. [Valid Anagram](https://leetcode.com/problems/valid-anagram/)
```js
// TC: O(n + m), SC: O(n)

const isAnagram = (s, t) => {
  const letters = {};
  
  for (let char of s) {
    letters[char] = ++letters[char] || 1;
  }
  
  for (let char of t) {
    letters[char] = --letters[char];
  }
  
  for (let freq of Object.values(letters)) {
    if (freq !== 0) return false;
  }
  
  return true;
}
```

## 2. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
- Use stack and brackets mapping to keep track of the expected closing bracket (if closing bracket).
```js
// TC: O(n), SC: O(n)

const isValid = (s) => {
  const map = {
    "(": ")",
    "[": "]",
    "{": "}"
  }

  const stack = [];
  
  for (let bracket of s) {
    // If bracket is an opening bracket, store its closing pair in the stack.
    if (map[bracket]) stack.push(map[bracket]);
    // If bracket is a closing bracket but the top stack is not that closing bracket, invalid.
    else if (bracket !== stack.pop()) return false;
  }
  
  return stack.length === 0;
}
```
