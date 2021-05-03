# Useful Tips

## Remainder/Modulo (%) Operator
> Returns the remainder left over when one operand is divided by a second operand. It always takes the sign of the dividend.
- Essentially, reduce the first operand to a maximum size of the second operand - 1.
  - Useful for limiting the range of the outcome.
- Notes
  - If the first operand is less than the second operand, the remainder will just be itself.
  - If the first operand is equal to the second operand, the remainder will be 0.
### Useful Scenarios
#### Even/Odd Number
```
if (x % 2 === 0) {} // if x is an even number.
if (x % 2 !== 0) {} // if x is an odd number.
```
#### Circular Array / Repeated Looping
- Repeated looping over the contents of the array.
  - When an array fills up (reached the end of the array), wrap back to the beginning and start filling back from the beginning.
  - Ex: allows us to store a certain number of most recent entries in the array.
- `[i % array.length]`
##### Example
- `i % studentN.length` loops the array back to the beginning infinitely.
- *i.e., if there is an array of 8 items, and we are currently on item 2 and we skip along 10 items, we want to end up back at slot 4, and not a non-existent slot 12.*
```
function solution(answers) {
    const student1 = [1, 2, 3, 4, 5];
    const student2 = [2, 1, 2, 3, 2, 4, 2, 5];
    const student3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];
    
    let student1Tally = 0;
    let student2Tally = 0;
    let student3Tally = 0;
    
    for (let i = 0; i < answers.length; i++) {
        if (student1[i % student1.length] === answers[i]) {
            student1Tally++;
        }
        if (student2[i % student2.length] === answers[i]) {
            student2Tally++;
        }
        if (student3[i % student3.length] === answers[i]) {
            student3Tally++;
        }
    }
    
    const answer = [];
    
    const max = Math.max(student1Tally, student2Tally, student3Tally);
    
    if (student1Tally === max) { answer.push(1) };
    if (student2Tally === max) { answer.push(2) };
    if (student3Tally === max) { answer.push(3) };
    
    return answer;
}
```

### Reference
[Remainder (%) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Remainder)
