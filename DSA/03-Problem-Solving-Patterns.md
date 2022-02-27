# Problem Solving Patterns

## Table of Contents
- [Frequency Counter](#frequency-counter)
- [Multiple Pointers](#multiple-pointers)
- [Sliding Window](#sliding-window)
- [Divide and Conquer](#divide-and-conquer)
- [Backtracking](#backtracking)
- [Greedy Algorithm](#greedy-algorithm)
- [Dynamic Programming](#dynamic-programming)
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
- **Create a window which can either be an array or number from one position to another (of some sequential data).**
  - Depending on a certain condition, the window either increases or closes (and a new window is created).
- Useful for keeping track of a subset of data that is **continuous** in some way in an array/string, etc.
### Types of Sliding Window
- Fixed-size Sliding Window
- Dynamically Resizable Window
  - The window can grow and shrink.
  - Like a caterpillar moving.
### How to Recognize a Problem to Apply the Sliding Window
- **Contiguous sequence of elements.**
  - These are elements that are grouped next to one another (subarray that occurs one after the other).
  - Look for words like "subarray", "substring", or any other words that refer to sequential grouping.
  - Can apply to strings, arrays, linked lists.
- **Context of the algorithm.**
  - When we need to calculate something.
  - Ex: if we need to find the min/max, shortest/longest, or if something is contained within the given dataset.
- **Some Question Variants**
  - Fixed-size Sliding Window
    - Ex: find the max sum of subarray of size `k`.
  - Dynamically Resizable Window
    - Ex: find the smallest sum of any window size that is greater than or equal to some value `k`.
  - Dynamically Resizable Window with Auxiliary Data Structure (we use an additional hashmap, hashset, array, etc.).
    - Ex: find the longest substring with no more than `k` distinct characters.
    - String Permutations
      - Ex: given strings `s` and `t`, does `t` exist as a permutation of `s`?
### Example 1 - fixed-size sliding window
- Write a function called `maxSubarraySum` which accepts an array of integers `arr` and a number called `num`. The function should calculate the maximum sum of `num` consecutive elements in `arr`.
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
// Implementation 1
function maxSubarraySum(arr, num) {
  // Edge case: if arr is less than num, return null.
  if (arr.length < num) return null;
  
  // Initilalize maxSum and tempSum variables.
  let maxSum = 0;
  let tempSum = 0;

  // Store the first combination in maxSum. We need to initialize first value to be able to implement a sliding window.
  for (let i = 0; i < num; i++) {
     tempSum += arr[i];
  }
  // Sync tempSum with maxSum.
  maxSum = tempSum;

  // Loop: starting from the number after the first combination.
  for (let i = num; i < arr.length; i++) {
    // Calculate next tempSum by subtracting the first number and adding the next number (sliding window).
    tempSum = tempSum - arr[i-num] + arr[i];
    // Compare new tempSum and maxSum, and update maxSum if necessary.
    if (tempSum > maxSum) maxSum = tempSum;
  }
  
  return maxSum;
}
```
```js
// Implementation 2
function maxSubarray(arr, num) {
  if (arr.length < num) return null;
  
  let maxSum = -Infinity;
  let tempSum = 0;
  
  for (let i = 0; i < arr.length; i++) {
    tempSum += arr[i];
    if (i >= k - 1) {
      maxSum = Math.max(maxSum, tempSum);
      tempSum -= arr[i - (k - 1)];
    }
  }
  
  return maxSum;
}
```
### Example 2 - fixed-size sliding window
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
### Example 3 - fixed-size sliding window
- Write a function called sumOddLengthSubarrays, which accepts an array of positive integers and calculates the sum of all possible odd-length subarrays.
#### Expectation
```js
sumOddLengthSubarrays([1,4,2,5,3]) // 58
sumOddLengthSubarrays([1,2]) // 3
sumOddLengthSubarrays([10,11,12]) // 66
```
#### Sliding Window Solution - TC: O(n<sup>2</sup>)
```js
function sumOddLengthSubarrays(arr) {
  // Initialize sum variable.
  let sum = 0;

  // Loop 1 (i simply represents the current odd length. Ex: 1,3,5,7,9)
  for (let i = 1; i <= arr.length; i+=2) {
      // Initialize currSum.
      let currSum = 0;

      // Loop 2 (0 to current odd length) to initialize the first sum of each odd subarray length (necessary to implement a sliding window).
      for (let j = 0; j < i; j++) {
          // Calculate currSum.
          currSum += arr[j];
      }

      // Update sum with currSum.
      sum += currSum;

      // Loop 3 (odd Length to arr.length) to add up the rest of Loop 1's iteration.
      for (let j = i; j < arr.length; j++) {
          // Calculate currSum by subtracting first number and adding next number (sliding window).
          currSum = currSum - arr[j-i] + arr[j];
          // Update sum with currentSum;
          sum += currSum;
      }
  }

  return sum;
}
```
#### Process Example
```js
sumOddLengthSubarrays([1,4,2,5,3])
// i = 1. Subarrays of length of 1.
  // currSum = 0.
  // currSum = 1.
  // sum = 1.
  // currSum = 1-1+4 = 4. sum = 1+4 = 5.
  // currSum = 4-4+2 = 2. sum = 5+2 = 7.
  // currSum = 2-2+5 = 5. sum = 7+5 = 12.
  // currSum = 5-5+3 = 3. sum = 12+3 = 15.
// i = 3. Subarrays of length of 3.
  // currSum = 0.
  // currSum = 1+4+2 = 7.
  // sum = 15+7 = 22.
  // currSum = 7-1+5 = 11. sum = 22+11 = 33
  // currSum = 11-4+3 = 10. sum = 33+10 = 43.
// i = 5. Subarrays of length of 5.
  // currSum = 0.
  // currSum = 1+4+2+5+3 = 15.
  // sum = 43+15 = 58.
  // Loop 3 doesn't run.
// 58
```
### Example 4 - dynamically resizable window
- Write a function called `smallestSubarrayGivenSum`, which accepts an array of integers and calculates the smallest size of a subarray that has a sum equal to or greater than a given sum.
#### Expectation
```js
smallestSubarrayGivenSum([4,2,2,7,8,1,2,8,1,0], 8) // 1
```
#### Sliding Window Solution
- If the sum of the window is less than the given sum.
  - Grow the window.
- Else if the sum is equal to or greater than the given sum.
  - Shrink the window.
  - Update the the smallest subarray size if necessary.
- Use index pointers (start and end) to maintain the window.
```js
function smallestSubarrayGivenSum(arr, targetSum) {
  let minWindowSize = Infinity;
  let currWindowSize = 0;
  let windowStart = 0;
  for (let windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    currWindowSize += arr[windowEnd];
    
    while(currWindowSum >= targetSum) {
      // `+1` to account for 0-indexed.
      minWindowSize = Math.max(minWindowSize, windowEnd - windowStart + 1);
      currWindowSize -= arr[windowStart];
      windowStart++;
    }
  }
  
  return minWindowSize;
}
```
### Example 5 - dynamically resizable window with auxiliary data structure
- Write a function called `longestSubstringKDistinct`, which accepts an array of characters and a natural number `k` to calculate the longest contiguous sequence of characters such that the number of distinct characters does not exceed `k`.
#### Expectation
```js
longestSubstring([A,A,A,H,H,I,B,C], 2) // 5
```
#### Sliding Window Solution
- Use a hashtable to keep track of the characters.
- Keep adding to the hashtable until the condition is violated.
- While the condition is violated, shrink the window and update hashtable count.
  - Remove a character from the hashtable if value is decremented to 0.
```js
function longestSubstringKDistinct(arr, k) {
  const charFreqMap = {};
  
  let maxLength = 0;
  
  let windowStart = 0;
  for (let windowEnd = 0; windowEnd < arr.length; windowEnd++) {
    charFreqMap[arr[windowEnd]] = ++charFreqMap[arr[windowEnd]] || 1;
    
    while (Object.keys(charFreqMap).length > k) {
      charFreqMap[arr[windowStart]]--;
      if (charFreqMap[arr[windowStart]] === 0) delete charFreqMap[arr[windowStart]];
      windowStart++;
    }
    maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
  }
  
  return maxLength;
}
```
### Reference
[Sliding Window Technique - Algorithmic Mental Models](https://www.youtube.com/watch?v=MK-NZ4hN7rs&ab_channel=RyanSchachte)

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

## Backtracking
> Backtracking is an algorithmic-technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, removing those solutions that fail to satisfy the constraints of the problem at any point of time (by time, here, is referred to the time elapsed till reaching any level of the search tree).
- For example, if there is one solution among three possible candidates, we have to check each solution one by one and discard non-solutions until the solution is found.
- *Backtracking algorithms are generally exponential in both time and space complexity.*
### Backtracking Flow
- The algorithm begins to build up a solution starting with an empty solution set to which moves are added one by one.
- Add the first remaining move to the solution set. This creates a new sub-tree in the search tree of the algorithm.
- If the sub-tree satisfies the constraints, then add more moves. Else, discard the entire sub-tree and recurse back to step 1 using the solution set.
- If the above check turns out to be the solution, output and terminate the program. Else, return that no solution is possible.
#### Example
- A tree has some good leaves and some bad leaves. At each node, beginning with the root, choose a child to move to and continue to do this until you reach a leaf.
- If you get a bad leaf, "backtrack" to continue the search for a good leaf by revoking your most recent choice, and trying out the next option in that set of options. If there are no more options, revoke the choice that got you there and try another choice at that node.
- If you end up back at the root with no options left, there are no good leaves to be found.
### Backtracking Pseudo Code
```js
// const solve = (node) => {
  // if n is a leaf node.
    // if the leaf is the target node, return true.
    // else return false.
  // else
    // for each child of node.
      // if solve(child) succeeds, return true.
    // return false.
// }
```
- Eventually, the recursion will "bottom" out at a leaf node.
- If `solve(node)` is `true`, it means that that `node` is part of a solution (i.e., it is one of the nodes on a path from the root to the target node.
- If `solve(node)` is `false`. it means that there is no path that includes `node` to the target node. Therefore, backtrack.
### Recursion and Backtracking
- A recursive function calls itself until it reaches a base case.
- Backtracking uses recursion to explore all the possibilities until the solution is reached.
### How to Recognize a Problem to Apply Backtracking
- Every constraint satisfaction problem with well-defined constraints on any objective solution that incrementally builds a candidate to the solution and abandons the candidate ("backtracks") as soon as it determines that the candidate cannot possibly be completed to a valid solution.
- Most problems can be solved using other algorithms like Dynamic Programming or Greedy Algorithms in logarithmic, linear, linear-logarithmic time complexity in order of input size. These algorithms usually perform better than Backtracking.
- ***Use Backtracking when there are multiple solutions and you want ALL those solutions.***
### Reference
[Backtracking | Introduction - GeeksforGeeks](https://www.geeksforgeeks.org/backtracking-introduction/)  
[Backtracking - UPENN](https://www.cis.upenn.edu/~matuszek/cit594-2012/Pages/backtracking.html)  

## Greedy Algorithm
- **An algorithm paradigm that follows the problem-solving method of selecting the locally optimal choice at each step in the hopes of achieving a global optimum.**
  - "Greedy" refers to the algorithm picking the locally optimal choice at each step to reach the global optimum solution.
- This algorithm is an optimization algorithm which can only find minimum or maximum possible result.
- Greedy algorithms are simple to develop and have a short execution time.
- However, they do not always produce a globally optimal result.
- Some applications of greedy algorithms include:
  - [Dijkstra's Algorithm](https://github.com/Kakamotobi/Learned/blob/main/DSA/18-Dijkstra's-Algorithm.md) - a graph search algorithm.
  - Huffman Coding
### Conditions to Use a Greedy Approach
1. Greedy Choice Property
    - Choosing the best option at each phase can lead to a global (overall) optimal solution.
2. Optimal Substructure
    - If an optimal solution to the total problem contains the optimal solutions to the sub-problems, the problem has an optimal substructure.
      - i.e., the problem can be split down into multiple subproblems and the solution to these subproblems can collectively lead to the solution for the entire problem.
### Limitations of Greedy Algorithms
- Greedy algorithms often fail because they do not consider all available data.
- A greedy algorithm may make its choices based on the choices it has made so far. But it is not aware of future choices that it could make.
- Dynamic Programming may be an appropriate solution for problems that greedy algorithms fail to solve successfully.
### Steps for Creating a Greedy Algorithm
- Find the best substructore/subproblem to the given problem.
- Determine what the solution will include (ex: largest sum, shortest path).
- Create an iterative process for traversing through local stages (subproblems) and making decisions to reach a global optimum solution.
### Reference
[Greedy Algorithms | Brilliant Math & Science Wiki](https://brilliant.org/wiki/greedy-algorithm/)  
[Greedy Algorithm | What is Greedy Algorithm? | Introduction To Greedy Algorithms | Simplilearn](https://www.youtube.com/watch?v=ilYwrsP7zzk)  

## Dynamic Programming
## Many more
