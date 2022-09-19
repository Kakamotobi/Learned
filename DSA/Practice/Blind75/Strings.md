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

## 6. [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
- Loop through each character in `s`.
  - Find the longest palindrome where the character is the center (expand from center).
    - *When checking for palindrome from the center, you need to handle an edge case: palindromes of even length.*
  - If the palindrome is longer than the existing longest palindrome substring, update it.
```js
// TC: O(n^3), SC: O(n)

const longestPalindrome = (s) => {
  let longestPalindromeString = "";
  
  let left = 0;
  let right = 0;
  
  for (let i = 0; i < s.length; i++) {
    // Check for palindromes of odd length.
    left = i;
    right = i;
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      if ((right - left + 1) > longestPalindromeString.length) {
        longestPalindromeString = s.slice(left, right + 1);
      }
      left--;
      right++;
    }
    
    // Check for palindromes of even length.
    left = i;
    right = i + 1;
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      if ((right - left + 1) > longestPalindromeString.length) {
        longestPalindromeString = s.slice(left, right + 1);
      }
      left--;
      right++;
    }
  }
  
  return longestPalindromeString;
}
```

## 7. [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
- Same approach as "Longest Palindromic Substring" problem.
```js
// TC: O(n^2), SC: O(n)

const countSubstrings = (s) => {
  let numPalindromes = 0;
  
  let left = 0;
  let right = 0;
  
  for (let i = 0; i < s.length; i++) {
    // Check for odd length palindromes.
    left = i;
    right = i;
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      numPalindromes++;
      left--;
      right++;
    }
    
    // Check for even length palindromes.
    left = i;
    right = i + 1;
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      numPalindromes++;
      left--;
      right++;
    }
  }
  
  return numPalindromes;
}
```

## 8. [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
- Use a hashmap to keep track of the anagrams that belong in the same group.
  - The key should represent the combination of alphabets by indicated the frequency of each of the 26 alphabets.
```js
// TC: O(n * k), SC: O(n)
  // n - number of elements in `strs`.
  // k - average number of characters in `strs[i]`.

const groupAnagrams = (strs) => {
  const map = {};
  
  for (let str of strs) {
    const combination = new Array(26).fill(0);
    
    // Record the frequency of each character.
    for (let char of str) combination[char.charCodeAt() - "a".charCodeAt()]++;
    
    const combinationKey = combination.join(",");
    
    map[combinationKey] = map[combinationKey] ? [...map[CombinationKey], str] : [str];
  }
  
  return Object.values(map);
}
```
