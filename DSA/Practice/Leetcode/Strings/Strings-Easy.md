# Strings - Easy

## 344. Reverse String
- Input: input string is given as an array of characters `s`.
- Output: reverse the array of characters in-place with O(1) extra memory.
### Example
```js
// Input: ["h","e","l","l","o"]
// Output: ["o","l","l","e","h"]
```
### Solution 1 - Iteration
```js
const reverseString = (s) => {
  let left = 0;
  let right = s.length - 1;
  while (left < right) {
    [s[left], s[right]] = [s[right], s[left]]
    left++;
    right--;
  }
  return s;
}
```
### Solution 2 - Recursion
```js
const reverseString = (s) => {
  function helper(left, right) {
    if (left >= right) return;
    else {
      [s[left], s[right]] = [s[right], s[left]];
      helper(++left, --right);
    }
  }
  helper(0, s.length - 1);
  return s;
}
```

## 242. Valid Anagram
- Input: two strings `s` and `t`.
- Output: return `true` if `t` is an anagram of `s`. Otherwise, return `false`.
- Constraints
  - `1 <= s.length, t.length <= 5 * 104`.
  - `s` and `t` consist of lowercase English letters.
### Solution
- Accumulate the frequency of the characters of string `s` in a hash table.
- Deduct each character of string `t` from string `s`'s frequency hash table.
- Return false if any frequency is not equal to `0`.
```js
const isAnagram = (s, t) => {
  const sFreq = {};
  for (let char of s) {
    sFreq[char] = ++sFreq[char] || 1;
  }
  
  for (let char of t) {
    sFreq[char]--;
  }
  
  for (let key in sFreq) {
    if (sFreq[key] !== 0) return false;
  }
  return true;
}
```
