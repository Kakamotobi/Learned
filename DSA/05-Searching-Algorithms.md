# Searching Algorithms

## Table of Contents
- [Linear Search](#linear-search)
- [Binary Search](#binary-search)
- [String Search](#string-search)
  - [Naive String Search](#naive-string-search)
  - [Knuth Morris Pratt (KMP) String Search](#knuth-morris-pratt-kmp-string-search)

## Linear Search
- **Given an array, the simplest way to search for a value is to look at every element in the array and check if it is the value that we want.**
- If the data is sorted, there are better ways to search than linear search.
- If the data is unsorted, linear search is the best we can do.
### Big O
- **TC: O(n)**
  - Best Case: O(1)
  - Average Case: O(n)
  - Worst Case: O(n)
### Some JS Array Methods that use Linear Search
- `indexOf`
- `includes`
- `find`
- `findIndex`
### Implementation
```js
function linearSearch(arr, val) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === val) return i;
  }
  return -1;
}
```

## Binary Search
- **Rather than eliminating one element at a time, you can eliminate half of the remaining elements at a time and narrow down to the search value.**
- Divide and Conquer.
  - Split the array into two pieces, pick a pivot point in the middle, check the left and right for where the data will be. Continue halving until found.
- *Very fast but only works on **sorted** arrays.*
### Big O
- **TC: O(log n)**
  - Best Case: O(1)
  - Average Case: O(log n)
  - Worst Case: O(log n)
- **Big O Calculation (log<sub>2</sub> n)**
  - **2 to what power gives us n?**
  - For the worst case scenario for 16 elements, it takes maximum 4 "steps".
    - log<sub>2</sub>16 = 4
  - For the worst case scenario for 32 elements, it takes maximum 5 "steps".
    - log<sub>2</sub>32 = 5
  - Doubling the length of the array will just add 1 more "step".
### Implementation
```js
function binarySearch(sortedArr, val) {
  let start = 0;
  let end = sortedArr.length - 1;
  let mid;
  
  while (left <= right) {
    mid = Math.floor((start + end) / 2);
    
    if (val < sortedArr[mid]) {
      end = mid - 1;
    }
    if (val > sortedArr[mid]) {
      start = mid + 1;
    }
    if (val === sortedArr[mid]) {
      return mid;
    }
  }
  
  return -1;
}
```

## String Search
- String/Pattern Search: finding all the occurences of a particular string in another string.
  - i.e., searching for substrings in a larger string.
### Naive String Search
- A straightforward approach involves checking pairs of characters individually.
#### Implementation
```js
function stringSearch(str, substr) {
  let count = 0;
  
  // Loop over str.
  for (i = 0; i < str.length; i ++) {
    // Loop over the substr.
    for (j = 0; j < substr.length; j++) {
      // If the characters don't match, break out of the inner loop. (i+j to check the proceeding chars in str1).
      if (str[i+j] !== substr[j]) break;
      // If the inner loop was able to be completed (found a match), increment the count.
      if (j === substr.length - 1) count++;
    }
    
    return count;
  }  
}

stringSearch("wzomgwomg", "omg") // 2
```
### Knuth Morris Pratt (KMP) String Search
- **Uses degenerating property of the pattern and improves the worst case complexity to O(n).**
  - Degenerating property means pattern having same sub-patterns appearing more than once in the pattern, are considered.
- **Instead of starting the search on each character of the main string, restart the matching at the index where the mismatch was encountered.**
  - How do we know that the match cannot be made anywhere in the first section?
  - The characters at the beginning of the pattern string itself need to be repeated for the possibility to exist.
  - If the characters at the beginning of the pattern string are not repeated in some other place of the pattern string, then it is safe to skip ahead on each mismatch.
#### Big O
- m: length of mainString.
- n: length of pattern String.
- LPS Array
  - TC: O(m)
  - SC: O(m)
- Matching Traversal
  - TC: O(n)
  - SC: O(1)
- Overall
  - **TC: O(m+n)**
  - **SC: O(m)**
#### Process
1) **Preprocess the pattern string before searching for it in the main string.**
    - Involves constructing an ***lps*** array corresponding to the pattern string of the same size as the pattern string.
      - **lps** refers to "longest prefix/suffix".
      - An array that stores the length of the longest prefix that occurs throughout the pattern string.
      - ***The lps array is used to calculate how far forward we can shift the pattern string.***
        - All about saving the progress.
        ```js
        index:         0 1 2 3 4 5 6 7 8 9
        pattern:       a b c x x x a b c y
        prefix length: 0 0 0 0 0 0 1 2 3 0
        
        // Prefix length tells us the length of the longest suffix that is also a prefix until that point.
        
        // Look for a pattern where the first character is "a", followed by the rest of the characters in the pattern string.
        // 0s mean that the longest prefix and suffix doesn't exist up to that point; hence, indicates the amount that we can shift forward.
        // Shift by = length of the pattern string at comparison point - corresponding value in the prefix length array.
        ```
        ```js
        // Example 2
        index:         0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17
        pattern:       a c a c a b a c a c a  b  a  c  a  c  a  c
        prefix length: 0 0 1 2 3 0 1 2 3 4 5  6  7  8  9  10 11 4 
                                              i                 j
                                 i                              j
                             i                                  j

        // At this point, "a" !== "c". So, i = table[i-1].
          // Get i's new position by getting the corresponding prefix/suffix length of the character right before. 11 - 1 = 10. Longest prefix/suffix length at that point is 5. So move i to index 5. Why? Because between index 0 and index 10, the longest prefix/suffix of length 5 means that there must be five characters for the prefix/suffix.
        // The prefix/suffix from index 0 to 4 and index 12 to 16 has already been matched. So, we need to compare index 5 and index 17.
        // At this point, "b" !== "c". So, i = table[i-1] again.
        // At this point, "c" === "c". So, table[j] = i + 1.
        ```

2) **Search the Pattern String in the Main String**
    - **Everytime a mismatch is encountered (with the main string), find out the longest suffix that is also a prefix in the pattern string before the mismatch.**
    - Example
      ```js
      // mainString = "adsgwadsxdsgwadsgz"
      // patternString = "dsgwadsgz"

      patternString: d s g w a d s g z
      lps array:     0 0 0 0 0 1 2 3 0

      // Compare "d" in patternString and "a" in mainString. Since this is a mismatch, move on to compare next character in mainString.

      // Mismatch with mainString occurs at index 7 ("g") on the patternString.
      // We have matched the patternString from index 0 to 6.
      // The longest prefix/suffix until that point is 2 ("ds").
      // This means that we can restart our search from index 2 of patternString with the mismatch point in mainString. We've prevented going backwards from our progress in mainString.

      // Now compare "g" (first g in patternString) in patternString and "x" in mainString.
      // Since, this is a mismatch, find the longest prefix that is also a suffix in patternString before the mismatch point.
      // The longest prefix/suffix until that point is 0.
      // This means that we have to restart our search from index 0 of patternString with the mismatch point in mainString.

      // Now compare "d" in patternString (first d in patternString) and "x" in mainString. Since this is a mismatch, move on to compare next character in mainString.

      // Now compare "d" (first d in patternString) and "d" in mainString.
      // Match is found.
      ```
#### Implementation
```js
// LPS Array

function lpsArray(patternStr) {
  const table = new Array(patternStr.length);
  table[0] = 0;
  let i = 0;
  let j = 1;
  
  while (j < table.length) {
    // If the values at i and j are equal.
    if (patternStr[i] === patternStr[j]) {
      // Set table value at position j to be i + 1.
      table[j] = i + 1;
      // Increment i and j.
      i++;
      j++;
    } 
    // Else if the values at i and j are not equal.
    else {
      // If value exists at index i - 1.
      if (i - 1 >= 0) {
        // Find the length of the longest prefix/suffix right before the mismatch at i's position.
        const valAtiMinus1 = table[i-1];
        // Move back i to that index. Why?? because it gives us the next longest prefix/suffix.
        i = valAtiMinus1;
      } 
      // If there is no value at index i - 1.
      else {
        // Set table value at position j to the index of i.
        table[j] = i;
        //Increment j.
        j++;
      }
    }
  }
  
  return table;
}
```
```js
// KMP Search

function kmpSearch(str, pattern) {
  // Get lps array.
  const table = lpsArray(pattern);
  
  // Pointer for str.
  let i = 0;
  // Pointer for table.
  let j = 0;
  
  while (i < str.length) {
    // If the values as i and j are equal.
    if (str[i] === pattern[j]) {
      // If j has reached the end of the table.
      if (j === pattern.length - 1) {
        // We found a match starting at index i - j.
        return i - j;
      }
      // Increment i and j.
      i++;
      j++;
    } 
    // Else if the values at i and j are not equal.
    else {
      // If value exists at j - 1. 
      if (j - 1 >= 0) {
        // Find the length of the longest prefix/suffix right before the mismatch at i's position.
        const valAtjMinus1 = table[j-1];
        // Move back j to that index.
        j = valAtjMinus1;
      }
      // If there is no value at j - 1.
      else {
        // Increment i.
        i++;
      }
    }
  }
  return -1;
}

kmpSearch("aaabbbccc", "bbb") // 3
kmpSearch("aaabbbccc", "xxx") // -1
kmpSearch("a", "a") // 0
kmpSearch("This is a test", " ") // 4
kmpSearch("", "a") // -1
```
#### Reference
[Knuth Morris Pratt (KMP) String Search Algorithm](https://www.youtube.com/watch?v=EL4ZbRF587g&ab_channel=StableSort)  
[Knuth-Morris-Pratt (KMP) Pattern Matching Substring Search](https://www.youtube.com/watch?v=BXCEFAzhxGY)
