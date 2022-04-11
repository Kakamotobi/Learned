# Basics

## Table of Contents
- [Programming vs Scripting vs Markup Language](#programming-vs-scripting-vs-markup-language)
- [Compiler vs Interpreter](#compiler-vs-interpreter)
- [Transpiler (Source-to-Source Compiler) and Polyfill](#transpiler-source-to-source-compiler-and-polyfill)
- [Typing](#typing)
- [ASCII and Unicode](#ascii-and-unicode)
- [Shallow vs. Deep Copy](#shallow-vs-deep-copy)

## Programming vs Scripting vs Markup Language
### Programming vs Scripting Language
|                | Programming Language | Scripting Language |
| -------------- | -------------------- | ------------------ |
| Definition     | Programming language is a set of instructions that can be fed into a computer to achieve a specific output. | Scripting language is a programming language supporting scripts written exclusively for a special runtime environment to automate a specific action/function execution. |
| Interpretation | Programming languages are compressed into smaller packages that do not need to be interpreted by another language or application. | ***Scripting languages are written in one language and interpreted within another program.*** For instance, JavaScript has to be incorporated within HTML, which the Internet browser will then interpret. |
| Execution      | Run independently of a parent program. | Run inside another program. |
| Memory         | When compiled, creates binary code executable files (.exe files) that take up memory. | Do not create executable files (i.e. don't take up memory). |
| Examples       | C, C++, C#, Java | JavaScript, Python, PHP, Ruby |

- *All scripting languages are programming languages.*
- ***A major difference between programming and scripting languages is that scripting languages are not required to be compiled, and are rather interpreted.***
  - For example, a C program needs to be compiled before running. Howevever, a Python program does not need to be compiled before running.
### Markup Language
- A type of computer language that uses tags to present the elements in a document and how they are structured.
- Ex: HTML, XML.
### Reference
[Computer Programming for Beginners | Programming, Scripting & Markup Languages | Ep19](https://www.youtube.com/watch?v=aExZPN6O_54&ab_channel=ProgrammingWithAvelx)  
[What's the difference between Scripting and Programming Languages? - GeeksforGeeks](https://www.geeksforgeeks.org/whats-the-difference-between-scripting-and-programming-languages/)  
[Difference Between Programming & Scripting Language - CN BLOG](https://www.codingninjas.com/blog/2018/12/08/difference-between-a-programming-language-and-a-scripting-language/)  

---

## Compiler vs Interpreter
> ...two different ways to translate a program from programming or scripting language to machine language.
- Most modern dynamic languages have implementations that use both interpreters and compilers.
- ***JavaScript used to be purely interpreted years ago. Now, it is JIT-compiled to native machine code in all major JavaScript implementations.***
### Compiler
- Source Code (high level lang.) &rarr; Compiler &rarr; Object Code (machine language a.k.a. binary code)
- Takes a bit of time to translate the whole source code all in one go. Once the translation is done, things will go fast.
- However, a mistake in the code will not be able to be corrected while translating.
- Issue with Precompiling
  - Compiling the source code and sending that compiled machine code to another computer can lead to errors. The computers may use different machine instructions because they have different operating systems or processor models.
### Interpreter
- Source Code (high level lang.) &rarr; Interpreter &rarr; Executable Code (machine language) &rarr; Get Next Instruction &rarr; Source Code
- It is a rather slow process as it translates the source code "line by line" communicating feedback.
- However, it gives you the chance to correct your mistakes as you go along.
### Compiler vs Interpreter
| Compiler  | Interpreter |
| --------- | ----------- |
| **Converts the entire source code (written in high level lang.) into executable machine code and then executes.**  | **Immediately executes and then translates a single statement of the source code into machine code before the next line is translated.**  |
| Generates a stand alone machine code program. | Performs the actions described by the high level program. |
| Takes extra preparation time, runs program very quickly. | Runs slowly, starts right away, let's you see how things are going. |
| Any errors are informed at the end of the compilation.  | Any error in the particular statement will terminate its translating process and display the error(s).  |
| The source code is translated to object code successfully if it is free of errors.  | The interpreter moves on only after the error has been removed.  |
### Just-In-Time(JIT) Compiler
- Unlike the Compiler where the translation is done ahead of time (i.e. before the code is executed), the JIT Compiler translates during execution.
- Every browser, and runtime in general, implements its own version of a JIT compiler.
- Source Code &rarr; Machine Code &rarr; Execute &rarr; Source Code
### Reference
[Language Processors: Assembler, Compiler and Interpreter](https://www.geeksforgeeks.org/language-processors-assembler-compiler-and-interpreter/)  
[The JIT in JavaScript: Just In Time Compiler](https://blog.bitsrc.io/the-jit-in-javascript-just-in-time-compiler-798b66e44143)  
[How do computers read code? - YouTube](https://www.youtube.com/watch?v=QXjU9qTsYCc&ab_channel=FrameofEssence)  
[COMPILER| INTERPRETER |Difference between Interpreter and Compiler| Interpreter vs Compiler Animated](https://www.youtube.com/watch?v=e4ax90XmUBc)  

---

## Transpiler (Source-to-Source Compiler) and Polyfill
### Transpiler
- A tool that converts a source code written in one programming language to source code in another language.
- Ex: Babel is a JavaScript transpiler which is usually used to convert ES6 code to previous version of JavaScript that is able to run on older JavaScript engines.
  - It parses ("read and understand") modern code and rewrite it using older syntax for it to work in older engines as well.
### Polyfill
- A polyfill is a script that updates and adds new functions.
> New language features may include not only syntax constructs and operators, but also built-in functions.  
> As we're talking about new functions, not syntax changes, there's no need to transpile anything here. We just need to declare the missing function. A polyfill "fills in" the gap and adds missing implementations.
- A polyfill emulates certain APIs, so that we can use them as though they were already implemented.
- Polyfill libraries: core-js, polyfill.io
### Reference
[Transpiler - devopedia](https://devopedia.org/transpiler)  
[Polyfills and transpilers - javascript.info](https://javascript.info/polyfills)  

---

## Typing
### Static Typing vs. Dynamic Typing
#### Static Typing
- Variable types are checked at "compile-time" or before runtime.
- So, compilation error will be thrown before the code runs.
- Ex: if a function receives a string as an argument, when in fact it's expecting a number, a compilation error is thrown.
#### Dynamic Typing
- Variable types are checked on the go as the code is executed.
- Ex: a function accepts any type of argument, and only knows which type it is when the code is executed.
### Strong Typing vs. Weak Typing
### Strong Typing
- Converts a variable or value's type to suit the current situation automatically.
- Ex: "123" will always be treated as a string (never used as a number unless otherwise manually converted).
- Python Example
  ```py
  x = 1
  y = "2"
  z = x + y # TypeError
  ```
### Weak Typing
- The interpreter or compiler attempts to make the best of what it is given.
- Ex: in some situations an integer miht be treated as if it were a string to suit the context of the situation.
- JavaScript Example
  ```js
  const x = 1;
  const y = "2";
  const z = x + y; // "12" because JS treated both values as strings and concatenated them together.
  ```
### Reference
[Understanding Types: Static vs. Dynamic, & Strong vs. Weak](https://medium.com/@cpave3/understanding-types-static-vs-dynamic-strong-vs-weak-88a4e1f0ed5f)

---

## ASCII and Unicode
### What is ASCII?
#### ASCII (0 - 127)
- American Standard Code for Information Interchange (ASCII) is a character encoding standard for electronic communication.
  - ASCII was meant for the English language.
- **ASCII is a 7-bit character set that represents a total of 2<sup>7</sup> = 128 characters.**
  - *The last bit (8th) is a parity bit (used for avoiding errors).*
  - ASCII control characters (0 - 31).
  - ASCII printable characters(32 - 127)
    - Some special characters.
    - Numbers from 0 - 9.
    - Upper and Lower Case Alphabets A - Z.
#### ASCII Extended (128 - 255)
- **ASCII Extended is an 8-bit character set that represents a total of 2<sup>8</sup> = 256 characters.**
- There are multiple variations of 8-bit ASCII tables as people started using the 8th bit to encode more characters to support other latin languages (ex: é).
  - Ex: ISO-8859-1.
### What is Unicode?
- ASCII Extended however, does not support other languages that are not latin-based.
- **The Unicode Standard is an information technology standard for the consistent encoding, representation and handling of text expressed in most of the world's writing systems.**
- Unicode is an abstract representation of the text, which needs to be encoded.
  - Encoding Formats
    - UTF-8: minimum 8 bits.
      - UTF-8 uses the ASCII set for the first 128 characters (i.e. ASCII text is valid in UTF-8).
    - UTF-16: minimum 16 bits.
    - UTF-32: minimum 32 bits.
### Reference
[ASCII Code - The extended ASCII table](https://www.ascii-code.com/)  
[What's the difference between ASCII and Unicode? - Stackoverflow](https://stackoverflow.com/questions/19212306/whats-the-difference-between-ascii-and-unicode)  

---

## Shallow vs. Deep Copy
### Shallow Copy
> Shallow Copy stores the references of objects to the original memory address. | GeeksforGeeks
- i.e., both the original and copy point to the same address in memory.
  - Therefore, any changes made to either the original or the copy will modify the other as well.
- The process of shallow copying is fast.
#### Example
```js
const original = [1,2,3,4];
const shallowCopy = original;

original.push(5);
shallowCopy.push(6);

console.log(original); // [1,2,3,4,5,6]
console.log(shallowCopy); // [1,2,3,4,5,6]
```
### Deepy Copy
> Deep copy stores copies of the object’s value. | GeeksforGeeks
- i.e., a different memory location is allocated for the copy (each has their own space in memory).
  - Therefore, any changes made to either the original or the copy will not reflect on the other.
- The process of deep copying is relatively slower and exhaustive.
#### Example
```js
const original = [1,2,3,4];
const deepCopy = [...original];

original.push(5);
deepCopy.push(6);

console.log(original); // [1,2,3,4,5]
console.log(deepCopy); // [1,2,3,4,6]
```
### Reference
[Difference between Shallow and Deep copy of a class - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-shallow-and-deep-copy-of-a-class/)  
[Understanding Deep and Shallow Copy in Javascript](https://medium.com/@manjuladube/understanding-deep-and-shallow-copy-in-javascript-13438bad941c)  

