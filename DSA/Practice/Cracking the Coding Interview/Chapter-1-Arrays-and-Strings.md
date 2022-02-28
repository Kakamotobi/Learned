# Chapter 1 - Arrays and Strings

## 1.3 URLify
- Inputs
  - A string `str`.
  - The "true" length of `str`.
- Output: the same string with all spaces replaced with "%20".
### Example
```js
URLify("Mr John Smith", 13) // "Mr%20John%20Smith"
```
### Solutions
#### Approach 1 - O(n)
- Make use of JavaScript String methods.
```js
const URLify = (str, trueLength) => {
  return str.trim().replaceAll(" ", "%20");
}
```
#### Approach 2 - O(n)
- Convert `str` to an array.
- Count the number of spaces in `str`.
- Compute the new length for the result: `trueLength + numSpaces * 2`.
  - `* 2` to account for only the additional "slots" that are needed.
- Reverse through the array (starting from the original last index).
  - If a space is encountered, replace it with "%20" (filling from the new last index).
  - Else copy the original character.
```js
const URLify = (str, trueLength) => {
  const arr = [...str];
  let numSpaces = 0;
  for (let char of arr) {
    if (char === " ") numSpaces++;
  }
  
  let newIndex = trueLength + numSpaces * 2;
  
  for (let i = trueLength - 1; i >= 0; i--) {
    if (arr[i] === " ") {
      arr[newIndex - 1] = "0";
      arr[newIndex - 2] = "2";
      arr[newIndex - 3] = "%";
      newIndex = newIndex - 3;
    } else {
      arr[newIndex - 1] = arr[i];
      newIndex--;
    }
  }
  
  return arr.join("");
}
```

## 1.4 Palindrome Permutation
- Input: a string `str`.
- Output: return true if `str` is a permutation of a palindrome. Otherwise, return false.
### Example
```js
isPermutationOfPalindrome("tact coa") // true
```
### Solution - O(n)
- Compute the frequencies of each character in a hash table.
- Count the number of odd frequencies.
- If there is more than one character that appears an odd number of times, return false. Otherwise return true.
  - *There can only be maximum one odd frequency for a string to be a palindrome.*
```js
const isPermutationOfPalindrome(str) => {
  const charFreq = {};
  for (let char of str) {
    charFreq[char] = ++charFreq[char] || 1;
  }
  
  if (" " in charFreq) delete charFreq[" "];
  
  let numOddFreq = 0;
  
  for (let freq of Object.values(charFreq)) {
    if (freq % 2 !== 0) numOddFreq++;
  }
  
  return numOddFreq <= 1;
}
```

## 1.5 One Away
- Inputs: two strings `str1` and `str2`.
- Output: return true if the two strings are one edit (or zero edits) away from being identical.
- Description
  - There are three types of edits that can be performed on strings.
    - Insert a character.
    - Remove a character.
    - Replace a character.
### Example
```js
isOneEditAway("pale", "ple") // true
isOneEditAway("pales", "pale") // true
isOneEditAway("pale", "bale") // true
isOneEditAway("pale", "bake") // false
```
### Solution - O(n)
- Since the condition is one edit or less,
  - If the lengths are the same, insertion/removal doesn't need to be considered.
    - Simply check whether the number of differences exceeds 1 or not (replacement).
  - Else if `str1` is longer than `str2` by one letter (one letter because any more than that exceeds the condition).
    - If adding one character to `str2` makes them identical, return true. Otherwise, return false.
  - Else if `str2` is longer than `str1` by one letter.
    - If adding one character to `str1` makes them identical, return true. Otherwise, return false.
```js
const isOneEditAway = (str1, str2) => {
  if (str1.length === str2.length) return isOneEditAwayReplace(str1, str2);
  else if (str1.length - 1 === str2.length) return isOneEditAwayInsert(str2, str1);
  else if (str2.length - 1 === str1.length) return isOneEditAwayInsert(str1, str2);
  return false;
}

// Check if the number of differences exceeds 1 or not.
const isOneEditAwayReplace = (s1, s2) => {
  let foundDifference = false;
  for (let i = 0; i < s1.length; i++) {
    if (s1[i] !== s2[i]) {
      if (foundDifference === true) return false;
    }
    foundDifference = true;
  }
  return true;
}

// Check if inserting a character into s1 makes it identical to s2.
const isOneEditAwayInsert = (s1, s2) => {
  let index1 = 0;
  let index2 = 0;
  while (index1 < s1.length && index2 < s2.length) {
    if (s1[index1] !== s2[index2]) {
      // If we had made a change already.
      if (index1 !== index2) return false;
      index2++;
    } else {
      index1++;
      index2++;
    }
  }
  return true;
}
```

