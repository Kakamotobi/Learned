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

## 3. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
- Filter out non-alphanumeric characters and then convert to lowercase.
- Use two pointers to check if palindrome.
```js
// TC: O(n), SC: O(n)

const parseString = (str) => {
  const re = /[a-zA-Z0-9]/g;
  const parsed = str.match(re);
  
  return parsed === null ? "" : parsed.toLowerCase();
}

const isPalindrome = (s) => {
  const parsedS = parseString(s);
  
  let left = 0;
  let right = parsedS.length - 1;
  
  while (left < right) {
    if (parsedS[left] !== parsedS[right]) return false;
    left++;
    right--;
  }
  
  return true;
}
```

## 4. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- Use sliding window to maintain duplicate-less substring.
  - Use a set for the window for quick lookup and delete.
```js
// TC: O(n), SC: O(n)

const lengthOfLongestSubstring = (s) => {
  let maxLength = 0;
  const window = new Set();
  let left = 0;
  
  for (let right = 0; right < s.length; right++) {
    // Remove and update the left end until the right end does not have a duplicate(s) in the substring.
    while (window.has(s[right])) {
      window.delete(s[left]);
      left++;
    }
    // Increase window.
    window.add(s[right]);
    // Update maxLength if necessary.
    maxLength = Math.max(maxLength, right - left + 1);
  }
  
  return maxLength;
}
```

## 5. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
### Solution 1
- In a given substring, all characters should match the most common character in that window (change less frequent characters to the most frequent character).
  - Therefore, find out the number of characters that needs to be replaced in a given substring.
    - substring length - frequency of the most frequent character.
    - The substring is valid if this number is less than or equal to `k`.
- Use a sliding window.
  - Maintain the substring using a hashmap where `character: frequency in the substring`.
  - If the substring is valid, increase window (increment right).
  - If invalid, shrink window (increment left).
```js
// TC: O(26*n), SC: O(n)

const characterReplacement = (s, k) => {
  let maxLength = 0;
  
  const window = {};
  
  let left = 0;
  for (let right = 0; right < s.length; right++) {
    // Update frequency.
    window[s[right]] = ++window[s[right]] || 1;
    
    // If window is invalid (need more than k operations).
    if (((right - left + 1) - Math.max(...Object.values(window)) <= k) === false) {
      window[s[left]]--;
      left++;
    }
  
    // Update maxLength if necessary.
    maxLength = Math.max(maxLength, right - left + 1);
  }
  
  return maxLength;
}
```
### Solution 2
- Same as Solution 1 but no need to go through the entire hashmap to get the largest frequency everytime.
```js
// TC: O(n), SC: O(n)

const characterReplacement = (s, k) => {
  let maxLength = 0;
  
  const window = {};
  
  let maxFreq = 0;
  
  let left = 0;
  for (let right = 0; right < s.length; right++) {
    // Update frequency.
    window[s[right]] = ++window[s[right]] || 1;
    
    // Update maxFreq if necessary.
    maxFreq = Math.max(maxFreq, window[s[right]]);
    
    // If window is invalid (need more than k operations).
    if (((right - left + 1) - maxFreq <= k) === false) {
      window[s[left]]--;
      left++;
    }
  
    // Update maxLength if necessary.
    maxLength = Math.max(maxLength, right - left + 1);
  }
  
  return maxLength;
}
```
