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

## 20. Valid Parentheses
- Input: a string `s` containing just the characters `"("`, `")"`, `"{"`, `"}"`, `"["`, `"]"`.
- Output: return true if the input string is valid.
  - An input string is valid if:
    - Open brackets must be closed by the same type of brackets.
    - Open brackets must be closed in the correct order.
### Solution - using stack
- If encountered an opening bracket, push the corresponding closing bracket to the stack.
- If encountered a closing bracket, pop from the stack and check to see if it is the correct closing bracket.
```js
const isValid = (s) => {
  const brackets = {
    "(": ")",
    "{": "}",
    "[": "]"
  }
  
  const stack = [];
  
  for (let char of s) {
    if (brackets[char]) stacks.push(brackets[char]);
    else {
      const topStack = stack.pop();
      if (char !== topStack) return false;
    }
  }
  
  return stack.length === 0;
}
```
