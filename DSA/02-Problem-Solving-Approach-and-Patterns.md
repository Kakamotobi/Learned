# Problem Solving Approach and Patterns

## Table of Contents
- [What is an Algorithm?](#what-is-an-algorithm)
- [Problem Solving Approach](#problem-solving-approach)
  - [Step 1: Understand the Problem](#step-1-understand-the-problem)
  - [Step 2: Explore Concrete Examples](#step-2-explore-concrete-examples)
  - [Step 3: Break it Down](#step-3-break-it-down)
  - [Step 4: Solve/Simplify](#step-4-solvesimplify)
  - [Step 5: Look Back and Refactor](#step-5-look-back-and-refactor)
- [Problem Solving Patterns](#problem-solving-patterns)
  - [Frequency Counter](#frequency-counter)
  - [Multiple Pointers](#multiple-pointers)
  - [Sliding Window](#sliding-window)
  - [Divide and Conquer](#divide-and-conquer)
  - [Dynamic Programming](#dynamic-programming)
  - [Greedy Algorithm](#greedy-algorithm)
  - [Backtracking](#backtracking)
  - [Many more](#many-more)

## What is an Algorithm?
- A **process** or **set of steps** to accomplish a certain task.
- The foundation for successfully solving problems and becoming a better developer.

## Problem Solving Approach
- Devise a plan.
### Step 1: Understand the Problem
1) **Can I restate the problem in my own words?**
    - Make sure to understand what the question is.
2) **What are the inputs that go into the problem?**
    - Think about the specific distinctions and conditions.
    - Ex: input size, input type, etc.
3) **What are the outputs that should come from the solution to the problem?**
4) **Can the outputs be determined from the inputs?**
    - Can I derive the expected outputs just using the inputs?
5) **How should I label the important pieces of data that are a part of the problem?**
    - What are the things that really matter in this problem and what is the terminology that I should use?
#### Example
- Write a function which takes two numbers and return their sum.
```js
// Step 1: Understand the Problem
// 1) Implement addition.
// 2) Two numbers.
//    - Integers? Floating points?
//    - How large are the numbers? Most languages have an upperbound where result becomes Infinity.
//    - What if someone leaves an input off?
//    - Strictly accept only two numbers? What if someone wants to add more than two numbers?
// 3) A single number.
//    - Similar questions as 2).
// 4) Yes. But,
//    - What if someone leaves an input off? Do we add 0? Do we return undefined or null?
// 5) function name: "add", input 1: "num1", input 2: "num2", returned result: "sum"
```
### Step 2: Explore Concrete Examples
- Coming up with examples can help you understand the problem better.
- Examples also provide sanity checks that your eventual solution works how it should.
1) **Start with simple examples.**
    - Input and expected corresponding output.
2) **Progress to more complex examples.**
3) **Explore examples with edge cases.**
    - Ex: empty inputs, invalid inputs
#### Example
- Write a function which takes in a string and returns counts of each character in the string.
```js
// Step 2: Explore Concrete Examples
// 1) Simple examples
//    - Input: "aaaa". Output: { a: 4 }
//      - Do we want only the character in the string return or should we include the rest of the alphabets as well? Ex: { a: 4, b: 0, c: 0 ... z: 0 }
//    - Input: "hello". Output: { h: 1, e: 1, l: 2, o: 1 }
// 2) More complex examples
//    - Input: "my phone number is 182736!"
//      - Should we account for spaces and other special characters?
//    - Input: "Hello hi"
//      - How about uppercase and lowercases? Are they separate counts?
// 3) Explore examples with edge cases.
//    - Input: "" or no argument or not a string?
//      - Should we return an empty object, null, undefined, error?
```
### Step 3: Break it Down
- **Explicitly write down the steps that you need to take.**
  - Doesn't have to be full pseudo-code or valid syntax. Just the basic components of the solution.
- This forces you to think about the code you'll write before you start writing it.
- Helps catch any lingering conceptual issues or misunderstandings before you dive in.
#### Example
- Write a function which takes in a string and returns counts of each character in the string.
```js
function charCount(str) {
  // Make object to return at end
  // Loop over string for each character
  //   If the char is a number/letter AND is already a key in object, add one to count.
  //   If the char is a number/letter AND is not in the object, add it to the object and set value to 1.
  //   If the char is something else, don't do anything.
  // Return object at end
};
```
### Step 4: Solve/Simplify
- Find the core difficulty in what you're trying to do.
- Temporarily ignore that difficulty.
- Write a simplified solution.
- Then, incorporate that difficulty back in.
#### Example
- Write a function which takes in a string and returns counts of each character in the string.
```js
function charCount(str) {
  // Make object to return at end
  const result = {};
  
  // Loop over string for each character
  for (let i = 0; i < str.length; i++) {
    const char = str[i].toLowerCase();
    // If the character already exists in the object, add one to count.
    if (result[char]) {
      result[char]++;
    } 
    // If the character doesn't exist in the object, add it to the object and set its value to 1.
    else {
      result[char] = 1; 
    };  
  };
  
  // Return object
  return result;
}
```
### Step 5: Look Back and Refactor
- Some questions to ask:
  - Can you check the result?
  - Can you derive the result differently?
  - Can you understand it at a glance?
  - Can you use the result or method for some other problem?
  - Can you improve the performance of your solution?
  - Can you think of other ways to refactor?
  - How have other people solved this problem?
#### Example
- Write a function which takes in a string and returns counts of each character in the string.
```js
// Before
function charCount(str) {
  const result = {};
  
  for (let i = 0; i < str.length; i++) {
    const char = str[i].toLowerCase();
    if (/[a-z0-9]/.test(char)) {
      if (result[char]) {
        result[char]++;
      } else {
        result[char] = 1; 
      };
    };
  };
  
  return result;
}

// Refactor 1
function charCount(str) {
  const result = {};
  
  for (let char of str) {
    char = char.toLowerCase();
    if (/[a-z0-9]/.test(char)) {
      result[char] = ++result[char] || 1;
    }
  }

  return result;
}

// Refactor 2
function charCount(str) {
  const result = {};
  
  for (let char of str) {
    if (isAlphaNumeric(char)) {
      char = char.toLowerCase();
      result[char] = ++result[char] || 1;
    }
  }

  return result;
}

function isAlphaNumeric(char) {
  const code = char.charCodeAt(0);
  if ((code > 47 && code < 58) || // numeric (0-9)
      (code > 64 && code < 91) || // upper alpha (A-Z)
      (code > 96 && code < 123)) { // lower alpha (a-z)
    return true;
  }
  return false;
}
```

## Problem Solving Patterns
### Frequency Counter
- **This pattern uses objects or sets to collect values and their frequencies.**
- ***Useful for algorithms with multiple pieces of data/inputs that need to be compared.***
- This can often avoid the need for nested loops or O(n<sup>2</sup>) operations with arrays/strings.
#### Example 1
- Write a function called 'same', which accepts two arrays. The function should return true if every value in the array has its corresponding value squared in the second array. The order doesn't matter but the frequency of values must be the same (i.e., arr1.length === arr2.length).
##### Expectation
```js
same([1,2,3], [4,1,9]) // true
same([1,2,3], [1,9]) // false (doesn't include the square of 2)
same([1,2,1], [4,4,1]) // false (the frequency must be the same)
```
##### Naive Solution - O(n<sup>2</sup>)
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
##### Frequency Counter Solution - O(n)
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
#### Example 2 - Anagram Challenge
- Given two strings, write a function to determine if the second string is an anagram of the first.
- An anagram is a word, phrase, or name formed by rearranging the letters of another, such as 'cinema', formed from 'iceman'.
##### Expectation
```js
validAnagram("", "") // true
validAnagram("hello", "hello") // true
validAnagram("aaz", "zza") // false
validAnagram("anagram", "nagaram") // true
validAnagram("awesome", "awesom") // false
validAnagram("qwerty", qeywrt") // true
validAnagram("chickenwings", "wingchickens") // true
```
##### Frequency Counter Solution - O(n)
```js
// inputs: str1, str2
// output: true/false
// length of both strings should equal

function validAnagram(str1, str2) {
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
```

### Multiple Pointers
### Sliding Window
### Divide and Conquer
### Dynamic Programming
### Greedy Algorithm
### Backtracking
### Many more
