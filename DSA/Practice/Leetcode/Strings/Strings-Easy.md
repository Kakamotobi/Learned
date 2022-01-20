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
