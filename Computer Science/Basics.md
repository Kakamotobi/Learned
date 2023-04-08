# Basics

## Table of Contents
- [Computer Basics](#computer-basics)
- [Programming vs Scripting vs Markup Language](#programming-vs-scripting-vs-markup-language)
- [Abstract Syntax Tree(AST)](#abstract-syntax-treeast)
- [Compiler vs Interpreter](#compiler-vs-interpreter)
  - [Just-In-Time(JIT) Compiler](#just-in-timejit-compiler)
- [Compile Time vs Runtime](#compile-time-vs-runtime)
- [Transpiler (Source-to-Source Compiler) and Polyfill](#transpiler-source-to-source-compiler-and-polyfill)
- [Typing](#typing)
- [ASCII and Unicode](#ascii-and-unicode)
- [Base64 Encoding/Decoding](#base64-encodingdecoding)
- [Shallow vs. Deep Copy](#shallow-vs-deep-copy)
- [Stream](#stream)

## Computer Basics
### Parts of Computer Hardware
- Input, CPU, Memory, Output.
#### Motherboard
- Handles the connection between the CPU and the Memory.
- Expansion slots
  - Put things that can improve the computer without putting more load on the CPU.
  - Ex: graphics card, sound card.
- Ports
  - A place on the motherboard to connect the CPU to some outside source (to get/give information).
  - Ex: USB, SD card, ethernet, audio.
### Types of Computers
#### Super Computer
- Parallel Processing
  - Uses multiple CPUs to handle separate parts of an overall task.
#### Server
- Stores/accesses data/programs.
#### Workstation
- Computers intended for individual use that is faster and more capable than a PC.
#### PC
- Computers that individuals use.
#### Microcontroller
- Tiny computers that specialize a specific task.

---

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

## Abstract Syntax Tree(AST)
> ...an **abstract syntax tree (AST)**, or just **syntax tree**, is a tree representation of the abstract syntactic structure of text (often source code) written in a formal language. Each node of the tree denotes a construct occurring in the text. | Wikipedia

- AST Expressing an expression in a tree structure.
- A branch from the root represents one whole expression.
### The Significance of AST
- An AST is agnostic to programming languages.
- AST is essentially about creating an intermediary abstraction.
  - All compilers require some sort of intermediary between the source code and compiled code, which are often in the shape of ASTs.
### Example
<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c7/Abstract_syntax_tree_for_Euclidean_algorithm.svg/800px-Abstract_syntax_tree_for_Euclidean_algorithm.svg.png"  alt="Abstract Syntax Tree" width="50%" style="background: white"/>
</div>

### Reference
[Abstract syntax tree - Wikipedia](https://en.wikipedia.org/wiki/Abstract_syntax_tree)  

---

## Compiler vs Interpreter
> ...two different ways to translate a program from programming or scripting language to machine language.
- Most modern dynamic languages have implementations that use both interpreters and compilers.
- ***JavaScript used to be purely interpreted years ago. Now, it is JIT-compiled to native machine code in all major JavaScript implementations.***
### [Compiler](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Compiler.md)
- Source Code (high level lang.) &rarr; Compiler &rarr; Object Code (machine language a.k.a. binary code)
- Takes a bit of time to translate the whole source code all in one go. Once the translation is done, things will go fast.
- However, a mistake in the code will not be able to be corrected while translating.
  - Check for syntax, semantic, and type.
- Issue with Precompiling
  - Compiling the source code and sending that compiled machine code to another computer can lead to errors. The computers may use different machine instructions because they have different operating systems or processor models.
### Interpreter
- Source Code (high level lang.) &rarr; Interpreter &rarr; Executable Code (machine language) &rarr; Get Next Instruction &rarr; Source Code
- It is a rather slow process as it translates the source code "line by line" communicating feedback.
- However, it gives you the chance to correct your mistakes as you go along.
### Compiler vs Interpreter
| Compiler  | Interpreter |
| --------- | ----------- |
| **Converts the entire source code (written in high level lang.) into executable machine code ahead of time (before the program runs).**  | **Immediately executes and then translates a single statement of the source code into machine code before the next line is translated.**  |
| Generates a stand alone machine code program. | Performs the actions described by the high level program. |
| Takes extra preparation time, runs program very quickly. | Runs slowly, starts right away, let's you see how things are going. |
| Any errors are informed at the end of the compilation.  | Any error in the particular statement will terminate its translating process and display the error(s).  |
| The source code is translated to object code successfully if it is free of errors.  | The interpreter moves on only after the error has been removed.  |
### Just-In-Time(JIT) Compiler
- Unlike the compiler where the translation is done ahead of time (i.e. before the code is executed), the JIT Compiler translates during execution.
- Source Code &rarr; Machine Code &rarr; Execute &rarr; Source Code
- Every browser, and runtime in general, implements its own version of a JIT compiler.
  - Chrome and Node.js uses V8.
  - Check [here](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Browsers.md#popular-web-browsers) for the JS engines that each browser uses for JIT compilation.
#### V8's JIT Compiler Structure
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/jit-compiler.png" alt="V8's JIT Compiler Structure" width="80%" /><br>
  <em>The inner workings of each engine is susceptible to change in efforts to further improve performance.</em>
</p>

##### 1) Parser and AST
- JavaScript code is **tokenized**.
  - Ex: `const num = 3;` can be tokenized as `const` (keyword), `num` (identifier), `=` (operator), `3` (number), `;` (semicolon).
- These tokens are used to create the **Abstract Syntax Tree (AST)**.
  - AST is a tree representing the syntactic structure of JavaScript code.
  - It is used to check for syntax, semantic errors.
  - It is also used to generate the actual machine code.
##### 2) Interpreter and Profiler
- JavaScript is interpreted by an **interpreter** called *Ignition*.
  - This interpreter quickly translates the source code to non-optimized machine code.
  - The execution starts with no delay.
- The **profiler** watches the code as the interpreter executes the code.
  - It keeps track of the number of times different statements are hit.
  - Hence, identifies areas for optimization.
    - These areas are passed on to the compiler.
##### 3) Compiler
- JavaScript is compiled by a **JIT optimizing compiler** called *TurboFan*.
- The compiler receives the areas for optimization from the profiler and translate them to machine code, replacing the corresponding code in the non-optimized machine code generated by the interpreter.
- The profiler and compiler constantly make changes to the machine code, effectively improving the JavaScript execution performance.
### Reference
[Language Processors: Assembler, Compiler and Interpreter](https://www.geeksforgeeks.org/language-processors-assembler-compiler-and-interpreter/)  
[How do computers read code? - YouTube](https://www.youtube.com/watch?v=QXjU9qTsYCc&ab_channel=FrameofEssence)  
[COMPILER| INTERPRETER |Difference between Interpreter and Compiler| Interpreter vs Compiler Animated](https://www.youtube.com/watch?v=e4ax90XmUBc)  
[The JIT in JavaScript: Just In Time Compiler](https://blog.bitsrc.io/the-jit-in-javascript-just-in-time-compiler-798b66e44143)  
[How Does JavaScript Really Work? (Part 1) | by Priyesh Patel | Bits and Pieces - Medium](https://blog.bitsrc.io/how-does-javascript-really-work-part-1-7681dd54a36d)  

---

## Compile Time vs Runtime
### Compile Time
- The time at which source code is translated to machine/binary code.
#### Compile Time Error
- Wrong syntax, wrong references to variables, functions, wrong types, etc. cannot successfully compile. Hence, lead to errors.
  - Ex: missing a `}`.
  - Ex: `3 + 2 = "I don't make sense"`.
- Errors are detected before the program is executed.
### Runtime
- The time at which a program is running (generally occurs after compile time).
#### Runtime Error
- Errors that are not detected by the compiler.
- Errors are detected after the program is executed (when the program is running).
- Mostly logical errors.
  - Ex: division by 0.
    - Most programming languages throw an exception at runtime.
    - But JavaScript is different. `0/0; // NaN`, `3/0; // Infinity`, `-3/0; // -Infinity`.
  - Ex: accessing a variable that has not been declared.
### Reference
[Difference between Compile Time Errors and Runtime Errors - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-compile-time-errors-and-runtime-errors/)  

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
- Variable types are checked at compile-time or before runtime.
  - Therefore, the script will not be compiled until there are no errors.
- Statically typed languages require the declaration of data types for all variables before using them.
- Ex: if a function receives a string as an argument, when in fact it's expecting a number, a compilation error is thrown.
- Ex: Java, Typescript (optional), C family.
#### Dynamic Typing
- Variable types are checked on the go as the code is executed (i.e. at runtime).
  - Therefore, the scripts can compile despite possibly having errors that will prevent successful execution.
- Dynamically typed languages are more flexible and efficient in writing scripts. But may lead to issues at runtime.
- Ex: a function accepts any type of argument, and only knows which type it is when the code is executed.
- Ex: JavaScript, Python.
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
[Dynamic typing vs. static typing - Oracle](https://docs.oracle.com/cd/E57471_01/bigData.100/extensions_bdd/src/cext_transform_typing.html#:~:text=There%20are%20two%20main%20differences,type%20checking%20at%20compile%20time.)  

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

## Base64 Encoding/Decoding
- Base64 encoding is a method that takes some binary data (Ex: colors and pixels of an image) and convert it to ASCII data (string of text).
- Commonly used when binary data needs to be encoded, especially when it needs to be stored and transferred over media that deal with text.
- Base64 encoding helps ensure that the data is not modified during transmission.
### Use Cases
- MIME.
- Storing complex data in JSON.
### Base64 Encoding/Decoding in JavaScript
- Encoding (binary to ASCII)
  - **`btoa()`**
    - Accepts a binary string (where each character is treated as a byte of binary data).
  - **`HTMLCanvasElement.toDataURL()`**
    - Takes the content of a canvas element and converts it to the Base64 encoding of that content.
- Decoding (ASCII to binary) - `atob()`
  - Accepts Base64 encoded data.
### Reference
[Base64 - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Base64)  

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

---

## Stream
> ...a **stream** is a sequence of data elements made available over time. | Wikipedia

- Items are processed one at a time instead of in batches.
- A stream may potentially have unlimited data.
- Standard Input/Output/Error are streams that a program can use to communicate with its environment.
- Examples
  ```js
  import { stdin, stdout } from "node:process";
  
  // Readable stream.
  stdin.on("data", (data) => {
    // ...
    // Writable stream.
    stdout.write(data);
  });
  ```
  ```js
  import * as readline from "node:readline/promises";
  import { stdin, stdout } from "node:process";
  
  // Interface for connecting `stdin` to `stdout`.
  const rl = readline.createInterface({ input: stdin, output: stdout });
  
  for await (const input of rl) {
    // ...
  }
  ```
### Reference
[Stream (computing) - Wikipedia](https://en.wikipedia.org/wiki/Stream_(computing))  
[Using stdout, stdin, and stderr in Node.js - LogRocket Blog](https://blog.logrocket.com/using-stdout-stdin-stderr-node-js/)  
[Streams API - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API)  
