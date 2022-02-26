# Basics

## Table of Contents
- [Typing](#typing)
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


