# Basics

## Table of Contents
- [Typing](#typing)
- [Compiler vs Interpreter](#compiler-vs-interpreter)
- [Transpiler (Source-to-Source Compiler) and Polyfill](#transpiler-source-to-source-compiler-and-polyfill)
- [ASCII and Unicode](#ascii-and-unicode)

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

## Compiler vs Interpreter
> ...two different ways to translate a program form programming or scripting language to machine language.
### Compiler
- Source Code (high level lang.) --> Compiler --> Object Code (machine language a.k.a. binary code)
- Generates a stand alone machine code program.
### Interpreter
- Source Code (high level lang.) --> Interpreter --> Executable Code (machine language) --> Get Next Instruction --> Source Code
- Performs the actions described by the high level program.
### Comparison
| Compiler  | Interpreter |
| ------------- | ------------- |
| **Converts the entire source code (written in high level lang.) into executable machine code and then executes.**  | **Immediately executes and then translates a single statement of the source code into machine code before the next line is translated.**  |
| Any errors are informed at the end of the compilation.  | Any error in the particular statement will terminate its translating process and display the error(s).  |
| The source code is translated to object code successfully if it is free of errors.  | The interpreter moves on only after the error has been removed.  |
### Note
- Most modern dynamic languages have implementations that use both interpreters and compilers.
- ***JavaScript used to be purely interpreted years ago. Now, it is JIT-compiled to native machine code in all major JavaScript implementations.***
### Just-In-Time(JIT) Compiler
- With JavaScript, compilation is done during execution.
- Every browser, and runtime in general, implements its own version of a JIT compiler.
- Source Code --> Machine Code --> Execute --> Source Code
### Reference
[Language Processors: Assembler, Compiler and Interpreter](https://www.geeksforgeeks.org/language-processors-assembler-compiler-and-interpreter/)  
[The JIT in JavaScript: Just In Time Compiler](https://blog.bitsrc.io/the-jit-in-javascript-just-in-time-compiler-798b66e44143)

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


