# Problem Solving Approach and Patterns

## Table of Contents
- [What is an Algorithm?](#what-is-an-algorithm)
- [Problem Solving](#problem-solving)
  - [Step 1: Understand the Problem](#step-1-understand-the-problem)
  - [Step 2: Explore Concrete Examples](#step-2-explore-concrete-examples)
  - [Step 3: Break it Down](#step-3-break-it-down)
  - [Step 4: Solve/Simplify](#step-3-solvesimplify)
  - [Step 5: Look Back and Refactor](#step-5-look-back-and-refactor)

## What is an Algorithm?
- A **process** or **set of steps** to accomplish a certain task.
- The foundation for successfully solving problems and becoming a better developer.

## Problem Solving
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
