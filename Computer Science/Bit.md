# Bit

## Table of Contents
- [What is it?](#what-is-it)
  - [Number of Permutations](#number-of-permutations)
- [Units of Memory Measurement](#units-of-memory-measurement)
- [Character Encoding](#character-encoding)
- [Types of Character Encoding Standards](#types-of-character-encoding-standards)
- [Bitwise Operation](#bitwise-operation)
  - [Binary Math](#binary-math)
  - [Bitwise Operators](#bitwise-operators)
  - [Bit Facts and Tricks](#bit-facts-and-tricks)
  - [JavaScript Bit Tips](#javascript-bit-tips)
- [Negative Binary](#negative-binary)
- [Signedness](#signedness)

## What is it?
- **A bit (a.k.a. binary digit) is the most basic unit of information that holds one of two values: 0 or 1.**
  - i.e. the smallest building block of storage.
- Computers count based on this binary unit (base 2).
  - cf. We count in base 10.
### Number of Permutations
- 1 bit holds 2^1 = 2 permutations (0 or 1).
- 2 bits hold 2^2 = 4 permutations.
- 3 bits hold 2^3 = 8 permutations.
- 4 bits hold 2^4 = 16 permutation.
- *A bit is too small to do anything substantial. Therefore, we group 8 bits and call it a **byte**.*
- 8 bits (1 byte) hold 2^8 = 256 possibilities.
  - 1 byte can represent one character.
  - This can express numbers from 0 to 255.

## Units of Memory Measurement
| Unit             | Memory     | Memory (bytes)              | Memory (bits)              | Number of Possible Permutations             |
| ---------------- | ---------- | --------------------------- | -------------------------- | ------------------------------------------- |
| Binary Digit     | 1 Bit      |               -             | 1 bit                      | 2<sup>1</sup> = 2                           |
| 1 Byte           | 8 Bits     | 1 byte                      | 8 bits                     | 2<sup>8</sup> = 256                         |
| 4 Byte           | 32 Bits    | 4 bytes                     | 32 bits                    | 2<sup>32</sup> = 4,294,967,296              |
| 8 Byte           | 64 Bits    | 8 bytes                     | 64 bits                    | 2<sup>64</sup> = 18,446,744,073,709,551,616 |
| 1 Kilobyte (KB)  | 1024 bytes | 1,024 bytes                 | 8,192 bits                 | 2<sup>8,192</sup>                           |
| 1 Megabyte (MB)  | 1024 KB    | 1,048,576 bytes             | 8,388,608 bits             | 2<sup>8,388,608</sup>                       |
| 1 Gigabyte (GB)  | 1024 MB    | 1,073,741,824 bytes         | 8,589,934,592 bits         | 2<sup>8,589,934,592</sup>                   |
| 1 Terabyte (TB)  | 1024 GB    | 1,099,511,627,776 bytes     | 8,796,093,022,208 bits     | 2<sup>8,796,093,022,208</sup>               |
| 1 Petabyte (PB)  | 1024 TB    | 1,125,899,906,842,624 bytes | 9,007,199,254,740,992 bits | 2<sup>9,007,199,254,740,992</sup>           |

## Character Encoding
- Character encoding is the process of mapping each character from a set of characters to a byte(s) in a computer, which allows them to be stored, transmitted and transformed.
  - Therefore, for example, if you press a character on the keyboard, the system's character encoding maps the character to its specific bytes in computer memory and then reads the bytes back into the character to display it.

## Types of Character Encoding Standards
- ASCII is a 7-bit character set was devised as a standard for the English language.
  - There came to be various character encoding standards for the various languages in the world as well.
- To accommodate more characters, ASCII Extended (an 8-bit character set) was introduced.
- To accommodate other languages that are not latin-based, Unicode was introduced.
  - Encoding formats of Unicode: UTF-8, UTF-16, UTF-32.
- [More on ASCII and Unicode here](https://github.com/Kakamotobi/Learned/blob/main/Computer%20Science/Basics.md#ascii-and-unicode)



## Bitwise Operation
- Bitwise operations refer to working with individual bits.
- A bitwise operation operates on two-bit patterns of equal lengths by positionally matching their invidual bits.
### Binary Math
| ... | 64s | 32s | 16s | 8s | 4s | 2s | 1s |
| --- | --- | --- | --- | -- | -- | -- | -- |
| ... | 1   | 0   | 0   | 0  | 0  | 0  | 0  |
#### Binary Addition
- `0 + 0 = 0`.
- `0 + 1 = 1`.
- `1 + 1 = 10`.
- `1 + 1 + 1 = 11`.
#### Binary Subtraction
- `1 - 0 = 1`.
- `1 - 1 = 0`.
- `10 - 1 = 1`. *Carrying over 1 to 0 will make it 2.*
### Bitwise Operators
#### Bitwise AND (`&`)
- Returns a `1` in each bit position for which the corresponding bits of both operands are `1`s.
- Example
  ```js
  const a = 5; // 0b101
  const b = 3; // 0b011
  
  console.log(a & b); // 0b001 // 1
  ```
#### Bitwise OR (`|`)
- Returns a `1` in each bit position for which the corresponding bits of either or both operands are `1`s.
- Example
  ```js
  const a = 5; // 0b101
  const b = 3; // 0b011
  
  console.log(a | b); // 0b111 // 7
  ```
#### Bitwise XOR (`^`)
- Returns a `1` in each bit position for which the corresponding bits of either but not both operands are `1`s.
- Example
  ```js
  const a = 5; // 0b101
  const b = 3; // 0b011
  
  console.log(a ^ b); // 0b110 // 6
  ```
#### Bitwise NOT (`~`)
- Inverts the bits of its operand.
- i.e., `0` becomes `1`, and `1` becomes `0`.
- Example
  ```js
  const a = 5; // 0b101
  const b = 3; // 0b011
  
  console.log(~a); // 1b101 // -6
  console.log(~b); // 0b010 // 2
  ```
#### Bitwise Shift
> A bit-shift moves each digit in a number's binary representation left or right.
##### Left Shift (`<<`)
- Shifts the first operand the second operand number of bits to the left.
  - The excess bits shifted off to the left are discarded.
  - Zero bits are shifted in from the right.
- *Shifting the original number left by one bit results to the original number * 2.*
  - Ex: `23 << 1` is `46`.
- Example
  ```js
  const a = 5; // 0b101
  const b = 2;
  
  console.log(a << b); // 0b10100 // 20 // Shift to the left `b` number of bits in the binary form of 5.
  ```
##### Arithmetic Right Shift (`>>`)
- Shifts the first operand the second operand number of bits to the right.
  - The excess bits shifted off to the right are discarded.
  - Copies of the leftmost bit is shifted in from the left.
- *Shifting the original number right by one bit results to the original number / 2.*
  - Ex: `23 >> 1` is `11` (truncated).
  - Ex: `-23 >> 1` is `12` (rounded up).
- Example
  ```js
  const a = 5; // 0b101
  const b = 2;
  const c = -5; // -0b101
  
  console.log(a >> b); // 0b001 // 1 // Shift to the right `b` number of bits in the binary form of 5.
  console.log(c >> b); // -0b010 // -2 // Shift to the right `b` number of bits in the binary form of -5.
  ```
##### Logical (Zero-fill) Right Shift (`>>>`)
- Shifts the first operand the second operand number of bits to the right.
  - The excess bits shifted off to the right are discarded.
  - Zero bits are shifted in from the left.
    - The sign bit becomes `0`, therefore, the result will always be positive.
    - Unlike the other bitwise operators, it returns an unsigned 32-bit integer.
- *For positive numbers only, shifting the original number right by one bit results to the original number / 2.*
  - Ex: `10 >>> 1` will result to `5`.
  - Ex: `-10 >>> 1` will result to `2147483643`.
- Example
  ```js
  const a = 5; // 0b101
  const b = 2;
  const c = -5; // -0b101
  
  console.log(a >>> b); // 0b001 // 1
  console.log(c >>> b); // 111111111111111111111111111110 // 1073741822
  ```
### Bit Facts and Tricks
#### AND (`&`)
- `X & 0s = 0`.
- `X & 1s = X`.
- `X & X = X`.
#### OR (`|`).
- `X | 0s = X`.
- `X | 1s = 1s`.
- `X | X = X`.
#### XOR (`^`)
- `X ^ 0s = X`.
- `X ^ 1s = ~X`.
- `X ^ X = 0`.
#### Other
- `X + X = X * 2 = X << 1`.
- `X * 4 = X << 2`.
- `X ^ (~X) = 1s`.
- `~0 = 1s`.
### JavaScript Bit Tips
- `Number.prototype.toString(2)` converts a number to its binary form.
  - The method accepts a radix/base representing the number of unique digits.

## Negative Binary
- Negative numbers are typically stored in *Two's Complement* form.
  - "Two's Complement" is the number that you have to add to the positive version of the number in binary to get 2<sup>x bit system</sup>
  - The left-most bit is used to represent the sign (`0` means a positive number, `1` means a negative number).
  - The remaining bits are filled in with the two's complement.
- `-X = inverse of X in binary + 1`.
### Example
| Base 10       | Base 2 (using two's complement) | Notes                                                              |
| ------------- | ------------------------------- | ------------------------------------------------------------------ |
| 18            | 00010010                        | The left-most bit is `0`, indicating that it is a positive number. |
| 2<sup>8</sup> | 10000000                        | Add the inverse of `0010010` (`1101101`) to `00100100`. This results to `01111111`. This means, adding `1` to `01111111`, hence, adding `1` to the inverse of `0010010` (`1101101`) and adding that to `00100100` will result to `10000000`. | 
| -18           | 11101110                        | The left-most bit is `1`, indicating that it is a positive number. |


## Signedness
> In computing, signedness is a property of data types representing numbers in computer programs. | Wikipedia
### Signed Integer vs Unsigned Integer
|            | Signed Integer | Unsigned Integer |
| ---------- | -------------- | ---------------- |
| Definition | Can represent both positive and negative numbers. | Can represent only non-negative numbers. |
| Example    | A signed 32-bit integer can hold the values from -2,147,483,648 to 2,147,483,647 (inclusive). | An unsigned 32-bit integer can hold the values from 0 to 4,294,967,295 (inclusive). |

## Reference
[Algorithms: Bit Manipulation - YouTube](https://www.youtube.com/watch?v=NLKQEOgBAnw)  
[Algorithms 101: the basics of Bit Manipulation explained](https://www.educative.io/blog/bit-manipulation-algorithm)  
[JavaScript Bitwise Operators (with Examples)](https://www.programiz.com/javascript/bitwise-operators)  
[What are bit-shift operations?](https://www.educative.io/edpresso/what-are-bit-shift-operations)  
[Bit Shifting (left shift, right shift) | Interview Cake](https://www.interviewcake.com/concept/java/bit-shift)  
[Signedness - Wikipedia](https://en.wikipedia.org/wiki/Signedness)  
