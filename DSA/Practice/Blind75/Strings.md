# Strings

## 1. [Valid Anagram](https://leetcode.com/problems/valid-anagram/)
```js
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
