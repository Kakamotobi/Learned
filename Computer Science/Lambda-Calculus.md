# Lambda(λ) Calculus

## Table of Contents
- [What is Lambda Calculus?](#what-is-lambda-calculus)
- [What is Lambda?](#what-is-lambda)
- [Lambda Notation](#lambda-notation)
- [Lambda Terms](#lambda-terms)
- [Relationship with Closure](#relationship-with-closure)

## What is Lambda Calculus?
> The Lambda calculus is an abstract mathematical theory of computation, involving λ functions. The lambda calculus can be thought of as the theoretical foundation of functional programming. It is a Turing complete language; that is to say, any machine which can compute the lambda calculus can compute everything a Turing machine can (and vice versa). | Brilliant

- It is a formal system for expressing computation based on abstraction and application.
- It was introduced in the 1930s by Alonzo Church as a declarative approach.
  - cf. The Turing Machine has an imperative approach.
- It consists of a single function (the **lambda function**).
  - _The lambda function is nameless_.
  - It is applied to one or more arguments, and returns only one expression (a value or a new function).
- In simple terms, the lambda function is an anonymous function. However, it provides the theoretical foundation for functional programming and other studies.

## What is Lambda?
- Lambda is a keyword that is used to define an anonymous function that takes one or more arguments and returns a value or a new function.

## Lambda Notation
### Example
- Function
  ```
  f(x) = x^2 + 1

  // or

  x -> x^2 + 1

  // or

  λx.x^2+1
  ```
- JS Equivalent
  ```js
  function(x) {
    return x^2 + 1;
  }

  // or

  (x) => x^2 + 1;
  ```

## Lambda Terms
1. **Variables**
   - If `x` is a variable, variable x is a lambda term pertaining to Λ.
2. **Abstractions**
   - If `x` is a variable and `M` is a lambda term, `(λx.M)` is a lambda term pertaining to Λ.
   - _`M` is where the lambda function is declared._
   - The variable `x` is bound in the expression.
3. **Applications**
   - If `M` pertains to Λ and `N` pertains to Λ, then `(M N)` is a lambda term pertaining to Λ.
     - i.e. the function `M` is applied to the argument `N`.

## Relationship with Closure
- A function is able to pertain access to variables in its lexical environment at the point of declaration, even after declaration through closure.
- A closure is created as a function returns a function.
- When returning the function, naming it is rather unnecessary. Therefore, lambda functions (anonymous and compact) were used.

## Reference
[Lambda Calculus | Brilliant Math & Science Wiki](https://brilliant.org/wiki/lambda-calculus/)  
