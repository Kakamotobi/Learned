# Compiler

## Table of Contents
- [What is a Compiler](#what-is-a-compiler)
- [Phases of a Compiler](#phases-of-a-compiler)
  - [1. Lexical Analysis](#1-lexical-analysis)
  - [2. Syntax Analysis](#2-syntax-analysis)
  - [3. Semantic Analysis](#3-semantic-analysis)
  - [4. Intermediate Code Generation](#4-intermediate-code-generation)
  - [5. Code Optimization](#5-code-optimization)
  - [6. Target Code Generation](#6-target-code-generation)
- [Compiler vs Interpreter](#compiler-vs-interpreter)
- [Just-In-Time(JIT) Compiler](#just-in-timejit-compiler)

## What is a Compiler?
- A compiler translates a program from one language (source code) to another language (machine code).
### Phases of a Compiler
<p align="center">
  <img src="https://www.guru99.com/images/1/020819_1119_PhasesofCom1.png" alt="Phases of a Compiler" width="100%"/><br/>
  <em>Each phase receives and works on the output of the previous phase.</em>
</p>

#### 1. Lexical Analysis
##### Tokenization
- A tokenizer is responsible for splitting the input stream into smaller units called tokens.
- A token simply referes to some defined unit that is meaningful to the compiler.
- Example
  - `const x = 1;` &rarr; `["const", "x", "=", "1", ";"]`
- Types of Tokens
  - **Identifier**
    - User defined names for variables, methods, etc.
    - Ex: `iAmVariable`, `funcThatDoesSomething`.
  - **Keyword**
    - A predefined/reserved word for the compiler.
    - Ex: `const`, `class`, `delete`, `if`, etc.
  - **Separator**
    - One or two tokens that separate language features.
    - Ex: `<`, `>`, `[`, `]`, `,` `;`.
  - **Operator**
    - A token that transforms one or more values/operands into a new value.
    - Ex: `+`, `=`, `<=`, `&&`.
  - **Literal**
    - A token with fixed values (immutable).
    - Ex: `true`, `null`, `2`, `"hello"`.
  - **Comment**
    - A token representing a line or block of comment.
    - Ex: `//`, `<--!iamcomment-->`
##### Lexer
- A lexer is responsible for analyzing each token's meaning.
  - Ex: identifying the data type (number, string, array, etc.), value of the token, and its children.
- Example
  - `["const", "x", "=", "1", ";"]` &rarr; `[{type: "keyword", value: "const"}, ..., {type: "number", value: 1}, ...]`
#### 2. Syntax Analysis
##### Parser
- A parser is responsible for identifying any syntax errors, and outputting an AST that a runtime environment can actually run/use.
  - An Abstract Syntax Tree(AST) is a tree representation of the abstract syntactic structure of text.
  - This outputted AST is a hierarchical representation of the source code where each node in the tree represents things like variable declarations, function calls, etc.
- It essentially prepares/structurizes some data to be further manipulated.
#### 3. Semantic Analysis
- In this phase, the AST is checked for any semantic errors (Ex: type mismatches, incompatible operands, undeclared variable, function call with improper arguments).
#### 4. Intermediate Code Generation
- An intermediate code of the program is generated in the target language.
- Typically a three-address code or a control flow graph.
#### 5. Code Optimization
- Make the intermediate code more efficient and faster to execute.
- Example of techniques used: constant folding, common subexpression elimination, instruction scheduling.
#### 6. Target Code Generation
- The final machine code is generated based on the above optimizations.

## Compiler vs Interpreter
> ...two different ways to translate a program from programming or scripting language to machine language.

- Most modern dynamic languages have implementations that use both interpreters and compilers.
- ***JavaScript used to be purely interpreted years ago. Now, it is JIT-compiled to native machine code in all major JavaScript implementations.***

| **Compiler**                                                                                                                            | **Interpreter**                                                                                                                          |
|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Source Code (high level lang.) → Compiler → Object Code (machine language a.k.a. binary code)                                           | Source Code (high level lang.) → Interpreter → Executable Code (machine language) → Get Next Instruction → Source Code                   |
| **Converts the entire source code (written in high level lang.) into executable machine code ahead of time (before the program runs).** | **Immediately executes and then translates a single statement of the source code into machine code before the next line is translated.** |
| Generates a stand alone machine code program.                                                                                           | Performs the actions described by the high level program.                                                                                |
| Takes extra preparation time, runs program very quickly.                                                                                | Runs slowly, starts right away, let's you see how things are going.                                                                      |
| Any errors are informed at the end of the compilation.                                                                                  | Any error in the particular statement will terminate its translating process and display the error(s).                                   |
| The source code is translated to object code successfully if it is free of errors.                                                      | The interpreter moves on only after the error has been removed.                                                                          |

## Just-In-Time(JIT) Compiler
- Unlike a static compiler where the translation is done ahead of time (i.e. before the code is executed), the JIT compiler translates during execution.
  - cf. static compiler
    - A static compiler cannot fully analyze and well optimize the code.
    - A JIT compiler has more observed information to make better optimizations.
- **A JIT compiler runs a program using an interpreter. While interpreting the program, the JIT compiler observes the executions and makes optimizations (compile an optimized version of something).**
  - Optimization Examples
    - Caching a frequently called function.
    - A function takes some parameters that are always integers.
  - Source Code → Machine Code → Execute → Possible Optimization → Source Code
    - _i.e. dynamic analysis and converting source code into machine code at runtime._
  - This means that JIT starts off slow but, hopefully, gets faster as optimizations take place.
- Every browser, and runtime in general, implements its own version of a JIT compiler.
  - Chrome and Node.js uses V8.
  - Check [here](https://github.com/Kakamotobi/Learned/blob/main/Web%20Development/Browsers.md#popular-web-browsers) for the JS engines that each browser uses for JIT compilation.
  - _Note: any given programming language can be implemented using an interpreter, compiler, JIT compiler, etc._
### V8's JIT Compiler Structure
<p align="center">
  <img src="https://raw.githubusercontent.com/Kakamotobi/Learned/main/Computer%20Science/refImg/jit-compiler.png" alt="V8's JIT Compiler Structure" width="80%" /><br>
  <em>The inner workings of each engine is susceptible to change in efforts to further improve performance.</em>
</p>

#### 1) Parser and AST
- JavaScript code is tokenized.
  - Ex: const num = 3; can be tokenized as const (keyword), num (identifier), = (operator), 3 (number), ; (semicolon).
- These tokens are used to create the **Abstract Syntax Tree (AST)**.
  - AST is a tree representing the syntactic structure of JavaScript code.
  - It is used to check for syntax, semantic errors.
  - It is also used to generate the actual machine code.
#### 2) Interpreter and Profiler
- JavaScript is interpreted by an **interpreter** called *Ignition*.
  - This interpreter quickly translates the source code to non-optimized machine code.
  - The execution starts with no delay.
- The **profiler** watches the code as the interpreter executes the code.
  - It keeps track of the number of times different statements are hit.
  - Hence, identifies areas for optimization.
    - These areas are passed on to the compiler.
#### 3) Compiler
- JavaScript is compiled by a JIT optimizing compiler called TurboFan.
- The compiler receives the areas for optimization from the profiler and translate them to machine code, replacing the corresponding code in the non-optimized machine code generated by the interpreter.
- The profiler and compiler constantly make changes to the machine code, effectively improving the JavaScript execution performance.

## Reference
[Phases of Compiler with Example: Compilation Process & Steps](https://www.guru99.com/compiler-design-phases-of-compiler.html)  
[Compiler vs Interpreter – Difference Between Them - Guru99](https://www.guru99.com/difference-compiler-vs-interpreter.html)  
[Language Processors: Assembler, Compiler and Interpreter](https://www.geeksforgeeks.org/language-processors-assembler-compiler-and-interpreter/)  
[How do computers read code? - YouTube](https://www.youtube.com/watch?v=QXjU9qTsYCc&ab_channel=FrameofEssence)  
[COMPILER| INTERPRETER |Difference between Interpreter and Compiler| Interpreter vs Compiler Animated](https://www.youtube.com/watch?v=e4ax90XmUBc)  
[Just In Time (JIT) Compilers - Computerphile | YouTube](https://www.youtube.com/watch?v=d7KHAVaX_Rs)  
[The JIT in JavaScript: Just In Time Compiler](https://blog.bitsrc.io/the-jit-in-javascript-just-in-time-compiler-798b66e44143)  
[How Does JavaScript Really Work? (Part 1) | by Priyesh Patel | Bits and Pieces - Medium](https://blog.bitsrc.io/how-does-javascript-really-work-part-1-7681dd54a36d)  
