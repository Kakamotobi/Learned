# Programming Paradigms

## Table of Contents
- [What are Programming Paradigms?](#what-are-programming-paradigms)
- [Declarative vs Imperative Programming](#declarative-vs-imperative-programming)

## What are Programming Paradigms?
- A style or a "way" of programming.
- Approaches to solve problems using some programming language.
- There are lots of programming languages but all of them need to follow some strategy when they are implemented and this methodology/strategy is paradigms.
- Some features that determine a programming paradigm are:
  - Modularity, objects, interrupts or events, control flow, etc.

## Declarative vs Imperative Programming
### Declarative Programming
- A type of programming paradigm that describes what programs to be executed.
- ***Declarative programs abstract the flow control process, and instead spend lines of code describing the data flow: What to do.***
- Concerned with the answer that is received more than the process.
  - Cannot trace the execution of the program as it runs.
- Focuses on end result.
- Think like a human.
  - i.e. "*What* is to be done."
- *Many (if not all) declarative APIs have some sort of underlying imperative implementation.*
- Ex: event handling code for a button can be encapsulated behind a button component, HTML, CSS.
- Ex: functional programming, logic programming.
#### Examples
##### Example 1
- The flow control is abstracted away by using the functional `Array.prototype.map()` utility, which allows you to more clearly express the flow of data.
```js
const doubleMap = numbers => numbers.map(n => n * 2);

console.log(doubleMap([2, 3, 4])); // [4, 6, 8]
```
##### Example 2
- Frontend frameworks (Ex: React) are declarative.
```js
let count = 0;

<button onClick={() => count++}>{count}</button>
```

### Imperative Programming
- A type of programming paradigm that describes how the program executes.
- ***Imperative programs spend lines of code describing the specific steps used to achieve the desired results — the flow control: How to do things.***
- Concerned with how to get an answer step by step.
  - Can trace the execution of the program as it runs.
- The order of execution is very important.
- Think like a computer.
  - i.e. "*How* it is to be done."
- Ex: object-oriented programming, procedural programming.
#### Examples
##### Example 1
- The flow control is explicitly expressed in step by step order.
```js
const doubleMap = numbers => {
  const doubled = [];
  for (let i = 0; i < numbers.length; i++) {
    doubled.push(numbers[i] * 2);
  }
  return doubled;
};

console.log(doubleMap([2, 3, 4])); // [4, 6, 8]
```
##### Example 2
- Using Vanilla JS to manipulate the DOM.
```js
const btn = document.querySelector("button");
let count = 0;

btn.eventListener(() => {
  count++;
  btn.innerText = count;
});

<button>0</button>
```
### Reference
[Difference Between Imperative and Declarative Programming - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-imperative-and-declarative-programming/)  
[Imperative vs Declarative Programming - YouTube](https://www.youtube.com/watch?v=E7Fbf7R3x6I&ab_channel=uidotdev)
