# Problem Solving Patterns

## Table of Contents
- [Frequency Counter](#frequency-counter)
- [Multiple Pointers](#multiple-pointers)
- [Sliding Window](#sliding-window)
- [Divide and Conquer](#divide-and-conquer)
- [Dynamic Programming](#dynamic-programming)
- [Greedy Algorithm](#greedy-algorithm)
- [Backtracking](#backtracking)
- [Many more](#many-more)

## Frequency Counter
- **This pattern uses objects or sets to collect values and their frequencies.**
- ***Useful for algorithms with multiple pieces of data/inputs that need to be compared (in particular, if they consist of the same individual pieces).***
- This can often avoid the need for nested loops or O(n<sup>2</sup>) operations with arrays/strings.
### Example 1
- Write a function called same, which accepts two arrays. The function should return true if every value in the array has its corresponding value squared in the second array. The order doesn't matter but the frequency of values must be the same (i.e., arr1.length === arr2.length).
#### Expectation
```js
same([1,2,3], [4,1,9]) // true
same([1,2,3], [1,9]) // false (doesn't include the square of 2)
same([1,2,1], [4,4,1]) // false (the frequency must be the same)
```
#### Naive Solution - TC: O(n<sup>2</sup>)
```js
// inputs: arr1, arr2
// output: true/false

function same(arr1, arr2){
  if (arr1.length !== arr2.length) return false;
  
  for (let i = 0; i < arr1.length; i++) {
    const correspondingIndex = arr2.indexOf(arr1[i] ** 2);
    if (correspondingIndex === -1) return false;
    arr2.splice(correspondingIndex, 1);
  }
  
  return true;
}
```
#### Frequency Counter Solution - TC: O(n)
```js
// Break down the arrays into objects comprised of the values and their corresponding frequencies.
// Then compare those objects.

function same(arr1, arr2) {
  // If lengths are different, return false.
  if (arr1.length !== arr2.length) return false;
  
  // To store the frequency of each individual values in each array.
  const freqCounter1 = {};
  const freqCounter2 = {};
  
  // Loop over arr1 and fill in freqCounter1.
  for (let val of arr1) {
    freqCounter1[val] = ++freqCounter1[val] || 1;
  }
  // Loop over arr2 and fill in freqCounter2.
  for (let val of arr2) {
    freqCounter2[val] = ++freqCounter2[val] || 1;
  }

  // Loop over freqCounter1 and compare with freqCounter2
  for (let key in freqCounter1) {
    // If the squared value doesn't exist (as a key in freqCounter2), return false.
    if (!(key ** 2 in freqCounter2)) return false;
    // If the frequencies do not match, return false.
    if (freqCounter2[key ** 2] !== freqCounter1[key]) return false;
  }

  return true;
}
```
### Example 2 - Anagram Challenge
- Given two strings, write a function to determine if the second string is an anagram of the first.
- An anagram is a word, phrase, or name formed by rearranging the letters of another, such as 'cinema', formed from 'iceman'.
#### Expectation
```js
validAnagram("", "") // true
validAnagram("hello", "hello") // true
validAnagram("aaz", "zza") // false
validAnagram("anagram", "nagaram") // true
validAnagram("awesome", "awesom") // false
validAnagram("qwerty", qeywrt") // true
validAnagram("chickenwings", "wingchickens") // true
```
#### Frequency Counter Solution - TC: O(n)
```js
// inputs: str1, str2
// output: true/false
// length of both strings should equal

// Approach 1 - two objects, three loops
function validAnagram(str1, str2) {
  if (str1.length !== str2.length) return false;

  // Initialize frequency counter object for both strings.
  const freqCounter1 = {};
  const freqCounter2 = {};
  
  // Loop over each string and record frequency of each character in each corresponding frequency counter object.
  for (let char of str1) {
    freqCounter1[char] = ++freqCounter1[char] || 1;
  }
  for (let char of str2) {
    freqCounter2[char] = ++freqCounter2[char] || 1;
  }
  
  // Loop over freq counter object of str1 and compare with freq counter object of str2.
  for (key in freqCounter1) {
    // If the value doesn't exist in freq counter object of str2, return false.
    if (!(key in freqCounter2)) return false;
    // If the frequencies don't match, return false. 
    if (freqCounter1[key] !== freqCounter2[key]) return false;
  }
  
  return true;
}

// Approach 2 - one object, two loops
function validAnagram(str1, str2) {
  if (str1.length !== str2.length) return false;
  
  // Initialize frequency counter object for str1.
  const freqCounter = {};
  
  // Loop over str1 and record frequency of each character in the frequency counter object.
  for (let char of str1) {
    freqCounter[char] = ++freqCounter[char] || 1;
  }
  
  // Loop over str2 and compare with freqCounter1.
  for (let char of str2) {
    // If char doesn't exist in freqCounter (or if frequency of char is 0), return false.
    if (!freqCounter[char]) return false;
    // Else if char exists in freqCounter, deduct its frequency.
    else --freqCounter[char];
  }
  
  return true;
}
```
### Example 3 - Checking for Duplicates
- Implement a function called, areThereDuplicates which accepts a variable number of arguments, and checks whether there are any duplicates among the arguments passed in.
#### Frequency Counter Solution - TC: O(n), SC: O(n) [vs Multiple Pointers Solution](#multiple-pointers-solution---tc-onlogn-sc-o1)
```js
function areThereDuplicates(...args) {
  // Initialize counter object.
  const freqCounter = {};
 
  // Loop through the arguments and tally up in the freqCounter object
  for (let arg of args) {
     freqCounter[arg] = ++freqCounter[arg] || 1;
  }
  
  // Check in freqCounter to see if any value is more than 1.
  for (let key in freqCounter) {
      if (freqCounter[key] > 1) return true;
  }
  
  return false;
}
```

## Multiple Pointers
- **Create pointers or values that correspond to an index or position and move towards the beginning, end, or middle based on a certain condition.**
- Very efficient for solving problems with minimal space complexity as well.
- The Idea:
  - Given some sort of linear structure (ex: string, array, singly or doubly linked list).
  - Search for a pair of values, or something that meets a condition.
- Use two references (ex: one at the beginning, one at the end) and work towards the middle.
### Example 1 - one pointer at beginning, one pointer at the end
- Write a function called sumZero which accepts a sorted array of integers. The function should find the first pair where the sum is 0. Return an array that includes both values that sum to zero or undefined if a pair does not exist.
#### Expectation
```js
sumZero([-3,-2,-1,0,1,2,3]) // [-3,3]
sumZero([-4,-3,-2,-1,0,1,2,5]) // [-2,2]
sumZero([-2,0,1,3]) // undefined
sumZero([1,2,3]) // undefined
```
#### Naive Solution - TC: O(n<sup>2</sup>), SC: O(1)
```js
// Input: ordered array of integers.
// Output: array of the pair of numbers that add up to 0. Else, undefined.

function sumZero(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i+1; j < arr.length; j++) {
      if (arr[i] + arr[j] === 0) return [arr[i], arr[j]];
    }
  }
  return undefined;
}
```
#### Multiple Pointers Solution - TC: O(n), SC: O(1)
```js
function sumZero(arr) {
  // Start with one pointer at the beginning and one at the end.
  let left = 0;
  let right = arr.length - 1;
  
  while (left < right) {
    // Add numbers at each point together.
    const sum = arr[left] + arr[right];
    // If the sum is 0, return the two numbers.
    if (sum === 0 ) return [arr[left], arr[right]];
    // If the sum is greater than 0 (meaning that there is no equivalent negative value), move the right pointer inwards.
    if (sum > 0) right--;
    // If the sum is less than 0, move the left pointer inwards.
    if (sum < 0) left++;
  }
  
  return undefined;
}
```
### Example 2 - two pointers at the beginning
- Write a function called countUniqueValues, which accepts a sorted array, and counts the unique values in the array. There can be negative numbers in the array, but it will always be sorted.
#### Expectation
```js
countUniqueValues([1,1,1,1,1,2]) // 2
countUniqueValues([1,2,3,4,4,4,7,7,12,12,13]) // 7
countUniqueValues([]) // 0
countUniqueValues([-2,-1,-1,0,1]) // 4
```
#### Multiple Pointers Solution - TC: O(n), SC: O(1)
```js
// Input: sorted array of integers.
// Output: the number of unique values.

// Approach 1 - mutate original array
function countUniqueValues(arr) {
  // If arr is empty, return 0.
  // Start pointers at the beginning (i, j). The second pointer being the "scout".
  // Compare the two pointers.
  //   If the number is the same, move j up once and start next comparison.
  //   If arr[i] !== arr[j], move i up once and then replace that value with the new number (build up unique values).
  
  if (arr.length === 0) return 0;
  
  let i = 0;
  
  for (let j = 1; j < arr.length; j++) {
    if (arr[i] !== arr[j]) {
      i++;
      arr[i] = arr[j];
    }
  }
  
  return i+1;
}

// Approach 2 - use a separate counter variable
function countUniqueValues(arr){
  // If arr is empty, return 0.
  // Initiate countOfUniqueValues = 1.
  // Set two pointers at the beginning of arr. The second pointer being the "scout".
  // if arr[pointer1] === arr[pointer2], pointer2++ and move on to next iteration.
  // if arr[pointer1] !== arr[pointer2], means that a unique value has been encountered.
  //   countOfUniqueValues++;
  //   set pointer1 = pointer2
  // When iteration is over, return countOfUniqueValues.
  
  if (arr.length === 0) return 0;
  
  let countOfUniqueValues = 1;
  
  let i = 0;
  
  for (let j = 1; j < arr.length; j++) {
      if (arr[i] !== arr[j]) {
          countOfUniqueValues++;
          i = j;
      }
  }
  
  return countOfUniqueValues;
}
```
### Example 3 - Checking for Duplicates
#### Multiple Pointers Solution - TC: O(nlog n), SC: O(1) [vs Frequency Counter Solution](#frequency-counter-solution-tc-on-sc-on)
```js
function areThereDuplicates(...args) {
  // Sort arguments
  args.sort();
  
  // Start pointers at the beginning, consecutively (index 0 and 1), of args.
  let i = 0;
  
  // Loop: second pointer being the scout.
  for (let j = 1; j < args.length; j++) {
    // If the the number that the second pointer is pointing to is equal to the number that the first pointer is pointing to, return true.
    if (args[i] === args[j]) return true;
    // Increment first pointer.
    i++;
  }

  return false;
}
```

## Sliding Window
- **Create a window which can either be an array or number from one position to another.**
  - Depending on a certain condition, the window either increases or closes (and a new window is created).
- Useful for keeping track of a subset of data that is **continuous** in some way in an array/string, etc.
### Example 1
- Write a function called maxSubarraySum which accepts an array of integers and a number called n. The function should calculate the maximum sum of n consecutive elements in the array.
#### Expectation
```js
maxSubarraySum([1,2,5,2,8,1,5], 2) // 10
maxSubarraySum([1,2,5,2,8,1,5], 4) // 17
maxSubarraySum([4,2,1,6], 1) // 6
maxSubarraySum([4,2,1,6,2], 4) // 13
maxSubarraySum([], 4) // null
```
#### Naive Solution - TC: O(n<sup>2</sup>)
```js
// Input: array of integers, number (representing the num of consecutive elements in the array that add up to the largest sum.
// Output: the largest sum

function maxSubarraySum(arr, num) {
  // If arr is less than num, return null.
  if (arr.length < num) return null;
  
  // Initialize maxSum to store max sum. -Infinity to account for if all numbers in arr are negative (in this case, starting it at 0 will always return 0).
  let maxSum = -Infinity;
  
  // Loop: until the last consecutive num combination.
  for (let i = 0; i < arr.length - num + 1; i++) {
    // Initialize tempMax to store temporary max.
    let tempMax = 0;
    // Loop: consecutive numbers.
    for (let j = 0; j < num; j++) {
      // Accumulate max in tempMax   
      tempMax += arr[i+j];
    }
    // Compare maxSum and tempMax and store the larger one.
    if (tempMax > max) max = tempMax;
  }
  
  return maxSum;
}
```
#### Sliding Window Solution - TC: O(n)
```js
// Rather than readding every single combination, subtract the first number and add the next number (effectively creating the next combination/window).

function maxSubarraySum(arr, num) {
  // Edge case: if arr is less than num, return null.
  if (arr.length < num) return null;
  
  // Initilalize maxSum and tempSum variables.
  let maxSum = 0;
  let tempSum = 0;

  // Store the first combination in maxSum.
  for (let i = 0; i < num; i++) {
     maxSum += arr[i];
  }
  // Sync tempMax with maxSum.
  tempSum = maxSum;

  // Loop: starting from the number after the first combination.
  for (let j = num; j < arr.length; j++) {
    // Calculate next tempSum by subtracting the first number and adding the next number (sliding window).
    tempSum = tempSum - arr[j-num] + arr[j];
    // Compare new tempSum and maxSum, and update maxSum if necessary.
    if (tempSum > maxSum) maxSum = tempSum;
  }
  
  return maxSum;
}
```
### Example 2
- Write a function called findLongestSubstring, which accepts a string and returns the length of the longest substring with all distinct characters.
#### Expectation
```js
findLongestSubstring("") // 0
findLongestSubstring("rithmschool") // 7
findLongestSubstring("thisisawesome") // 6
findLongestSubstring("thecatinthehat") // 7
findLongestSubstring("bbbbbb") // 1
```
#### Sliding Window Solution - TC: O(n)
```js
function findLongestSubstring(str){
    // Initialize longestLength = 0.
    let longestLength = 0;
    // Initialize start.
    let start = 0;
    // Initialize already seen object with values as the next character's index (start of new window).
    let alreadySeen = {};
    
    // Loop: i represents the end of the window.
    for (let i = 0; i < str.length; i++) {
        let char = str[i];
        // If char exists in alreadySeen, move start point to the next character of the first of the duplicate.
        if (alreadySeen[char]) {
            start = Math.max(start, alreadySeen[char]);
        }
        // Update the longestLength if necessary. +1 because index is 0-based.
        longestLength = Math.max(longestLength, i - start + 1);
        // Record the index/location of the next character (new start of window).
        alreadySeen[char] = i + 1;
    }
    
    return longestLength;
}
```

## Divide and Conquer
- **Involves dividing a data set (array, string, etc.) into smaller chunks and then repeating a process with a subset of data.**
- Can tremendously decrease time complexity.
- Some Divide and Conquer Algorithms
  - Quick Sort
  - Merge Sort
  - Binary Search
### Example
- Given a sorted array of integers, write a function called search, that accepts a value and returns the index where the value assed to the function is located. If the value is not found, return -1.
#### Expectation
```js
search([1,2,3,4,5,6], 4) // 3
search([1,2,3,4,5,6], 6) // 5
search([1,2,3,4,5,6], 11) // -1
```
#### Naive Solution - TC: O(n)
```js
// Linear Search

function search(arr, num) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === num) return i;
  }
  return -1;
}
```
#### Divide and Conquer Solution - TC: O(log n)
```js
// Binary Search

function search(arr, num) {
  let low = 0;
  let high = arr.length - 1;
  
  while (low <= high) {
    let mid = Math.floor((low + high) / 2);
    
    if (num > arr[mid]) {
      low = mid + 1;
    }
    else if (num < arr[mid]) {
      high = mid - 1;
    }
    else {
      return mid;
    }
  }
  
  return -1;
}
```

## Dynamic Programming
## Greedy Algorithm
## Backtracking
## Many more
