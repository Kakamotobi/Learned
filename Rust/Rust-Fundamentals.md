# Rust Fundamentals

## Table of Contents
- [Rust](#rust)
- [Cargo](#cargo)
- [Variables](#variables)
  - [Immutability](#immutability)
  - [`let`](#let)
  - [`const`](#const)
- [Scope](#scope)
- [Memory Safety](#memory-safety)
- [Functions](#functions)
- [Module System](#module-system)

## Rust
- A systems programming language that focuses on three goals: safety, speed, concurrency.
- Rust is strongly typed and can infer appropriate types where possible.
- Rust provides low-level capabilities with higher levels of abstractions at zero cost.
- Rust enforces memory safety (i.e. all references point to valid memory) without a garbage collector.

## Cargo
- Rust's package manager, build system, test runner, documentation generator, etc.
- `cargo run`
  - Compile project and outputs the build in the `target` folder.
  - By default compiles to `debug` symbol (Ex: `target/debug/hello`).
  - `cargo run --release` compiles to `release` symbol (Ex: `target/release/hello`).
- Cargo uses the "rustc" compiler.

## Variables
### Immutability
- **Variables are immutable by default.**
  - Safety - less bugs since a value will never change.
  - Concurrency - data can be shared between multiple threads without locks since it is immutable.
  - Speed - the compiler can confidently make optimizations since data is immutable.
- **To make them mutable, you have to explicitly declare them to be mutable using `mut`.**
  - Ex: `let mut count = 0;`
### `let`
```rs
let num: i32 = 7; // signed 32-bit integer
```
- `let` statement can destructure data on the right hand side.
  ```rs
  let (num1, num2) = (7, 10);
  ```
### `const`
- Convention: screaming-snake-case.
- Rules
  - Type annotation is required.
  - The value must be a constant expression that can be determined at compile time.
- Example
  ```rs
  const MY_NAME: char = "Kakamotobi";
  ```
- Why use `const`?
  - It can be used as an immutable, global constant.
    - i.e. the variable will never change during the the program's execution.
  - Since `const`s are inlined at compile time (rather than accessing the value - Ex: function call, variable lookup - at runtime), they are very fast.

## Scope
- The "place" in the code that a particular variable can be used in.
- **A variable's scope begins where it is created until the end of the block (including nested blocks).**
- **Since there is no garbage collector, variables are immediately dropped from memory at the end of its scope.**

## Memory Safety
- Memory safety is guaranteed at compile time.
- Variables must be initialized before it can be used.
  - Example
    ```rs
    let num: i32;
    println!("num is {}", num); // Compile Error
    ```
    ```rs
    let num: i32;

    if true {
      num = 1;
    }

    println!("num is {}", num); // Compile Error
    ```
    ```rs
    let num: i32;

    if true {
      num = 1;
    } else {
      num = 2;
    }
    
    println!("num is {}", num); // No Error
    ```

## Functions
- Convention: snake-case for function names.
- Functions are hoisted (i.e. can be called before being declared).
- Example
  ```rs
  fn add_nums(num_a: i32, num_b: i32) -> i32 {
    return num_a + num_b;
    // OR
    num_a + num_b // tail expression (no `return` keyword and semicolon)
  }
  ```
- A function cannot have variable numbers of arguments or different types for the same argument.
  - cf. macros such as `println` do.
    - Macros always ends with an exclamation mark (`println!()`).

## Module System
- `main.rs` is the binary.
- `lib.rs` is the library.
- All items in a library are private by default; even to binaries in the same project.
  - Use the `pub` keyword to indicate a function to be public.
  - `<library name>::<function name>()`
    - `<library name>`: the same as the project name as specified in Cargo.toml.
    - `::`: the scope operator, which
    - `<function name>`: the library's function to be called.
  - Example
    ```rs
    // main.rs
  
    fn main() {
      hello::greet(); // directly call the library's function by specifying the absolute path to the function.
    }
    ```
    ```rs
    // lib.rs
  
    pub fn greet() {
      println!("Hi!");
    }
    ```
- Use the `use` keyword to bring an item from some path into some scope (i.e. importing).
  - Example
    ```rs
    // main.rs

    use hello::greet;
    use rand::prelude::*;

    fn main() {
      greet();

      let x: f64 = thread_rng().gen();
      println!("x is {}", x);
    }
    ```
  - The standard library(`std`) is always available by default.
- "crates.io" is Rust's package registry (collection of things not in the standard library).
