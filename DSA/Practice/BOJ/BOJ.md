# BOJ

## Table of Contents
- [BOJ 1009](#boj-1009)
- [BOJ 1076](#boj-1076)

## [BOJ 1009](https://www.acmicpc.net/problem/1009)
### Attempt 1
- Fails when `numData > Number.Max_SAFE_INTEGER`.
  - JS loses precision beyond `Number.Max_SAFE_INTEGER`(2^53 - 1) since values above it cannot be accurately represented in memory.
- Testcase Errors
  - `7^100 > Number.Max_SAFE_INTEGER`
  - `9^635 > Number.Max_SAFE_INTEGER`

```js
const fs = require("fs");
const input = fs.readFileSync("/dev/stdin").toString().split("\n");

for (let i = 1; i <= input[0]; i++) {
  const [a, b] = input[i].split(" ");
  const numData = a ** b;
  const lastDigit = numData % 10;

  console.log(lastDigit === 0 ? 10 : lastDigit);
}
```
### Solution
- No need to know the total number of data.
- Only need to know the last digit to find out the last digit of the next multiplication.
```js
const fs = require("fs");
const input = fs.readFileSync("/dev/stdin").toString().split("\n");

for (let i = 1; i <= input[0]; i++) {
  const [a, b] = input[i].split(" ");

  let lastDigit = 1;
  for (let j = 0; j < b; j++) {
    lastDigit = (lastDigit * a) % 10;
  }

  console.log(lastDigit === 0 ? 10 : lastDigit);
}
```
## [BOJ 1076](https://www.acmicpc.net/problem/1076)
- Use a hashmap to map colors to their corresponding values.
```js
const fs = require("fs");
const input = fs.readFileSync("/dev/stdin").toString().split("\n");

const colors = {
  black: 0,
  brown: 1,
  red: 2,
  orange: 3,
  yellow: 4,
  green: 5,
  blue: 6,
  violet: 7,
  grey: 8,
  white: 9,
};

const [color1, color2, color3] = input;

const resistance =
  parseInt(`${colors[color1]}${colors[color2]}`) * 10 ** colors[color3];
console.log(resistance);
```
