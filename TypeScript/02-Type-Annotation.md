# Type Annotation
- "This variable is a string."
- "This function should return a boolean."
- "This function accepts two numbers and returns a number."
- "This object must have a property called colors, set to an array of strings."

## Table of Contents
- [Type Inference](#type-inference)
- [Type Annotation for Variables](#type-annotation-for-variables)

## Type Inference
- The TypeScript compiler can look at a variable declaration and determine what type should be inferred for the variable's value.
- It can remember a value's type even without providing a type annotation to it, and enforces that type on that value.
- ***Due to type inference, you can choose to simply annotate variables that are initialized first then its value is declared later.***
  - If a value-less variable is initialized without being annotated, its type is set to "any".
### Example
```ts
let num = 3;
num = "three"; // ERROR - Type 'string' is not assignable to type 'number'.
```

## Type Annotation for Variables
- Syntax: `let myVar: type = value`
### Examples
```ts
let str: string = "I am a variable";
let num: number = 3;
let isActive: boolean = true;
```
```ts
let mixed: any = "I am a string";
mixed = 3;
mixed = true;
mixed = [];
mixed = {};
mixed();
```
