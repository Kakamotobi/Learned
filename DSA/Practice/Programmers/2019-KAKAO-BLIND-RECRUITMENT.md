# 2019 KAKAO BLIND RECRUITMENT

## [실패율](https://programmers.co.kr/learn/courses/30/lessons/42889)
- **Inputs**
  - A natural number `N` representing the total number of stages.
  - An array of natural numbers `stages` where `stages[i]` represents the current stage that a user is currently at.
- **Output:** return an array consisting of the stage numbers in decreasing order of failure rate (stage with highest failure rate to stage with lowest failure rate).
  - Failure Rate = Number of users that have arrived at the stage but haven't cleared the stage yet / Number of users that have arrived (those that have passed or are at) at the stage.
- **Constraints**
  - `1 <= N <= 500`.
  - `1 <= stages.length <= 200,000`.
  - `1 <= stages[i] <= N + 1`.
    - `N + 1` represents users that have cleare the last (N<sup>th</sup>) stage.
  - If the failure rates are identical, let the earlier stage come first.
  - If no user is at a stage, the failure rate is 0.
### Example
```js
solution(5, [2,1,2,6,2,4,3,3]); // [3,4,2,1,5]
solution(4, [4,4,4,4,4]); // [4,1,2,3]
```
### Solution
- **TC: O(n)**
  - n: the number of elements in `stages`.
- Use a hash table to record the number of users at each stage.
- Calculate the failure rate for each stage.
  - Failure Rate = # of users at the stage / # of users at stages that are greater than or equal to the stage.
  - \# of users at stages that are greater than or equal to the stage = Total # of Users - # of Users at the previous stage.
    - Ex: # of Users that have attempted (passed & stuck at) Stage 2 = Total # of Users - # of Users at Stage 1.
    - If `stages.length` is `8`, all `8` users have been at Stage 1. Therefore, the number of users at Stage 2 would be `8 - the number of users that are still at Stage 1`.
- Sort the stages by failure rate in decreasing order.
```js
const solution = (N, stages) => {
  const obj = {};
  
  // Record the number of users at each stage.
  for (let stage of stages) {
    if (stages <= N) {
      obj[stage] = ++obj[stage] || 1;
    }
  }
  
  let numAttemptedUsers = stages.length;
  // Calculate the failure rates of each stage and replace values in obj.
  for (let i = 1; i <= N; i++) {
    let temp = obj[i];
    obj[i] = obj[i] / numAttemptedUsers || 0; // Failure rate should be 0 if there are no users at the stage.
    if (temp) numAttemptedUsers -= temp;
  }
  
  // Collect the map keys in an array.
  const keys = Object.keys(obj);
  // Sort the keys by their values in decreasing order.
  keys.sort((a,b) => obj[b] - obj[a]);
  
  // Convert the keys back to Numbers.
  for (let i = 0; i < keys.length; i++) {
    keys[i] = parseInt(keys[i]);
  }
  
  return keys;
}
```
#### Same Solution Using `Map`
- The above solution can be implemented using JavaScript's `Map`. However, `Map` stores data in insertion order of the keys.
- So, if `stages = [2,1,2,6,2,4,3,3]`, the hash table will be `{2 => 3, 1 => 1, 4 => 1, 3 => 2}` for the number of users at each stage.
  - `2` is the first in the hash table because it is the first number that was inserted.
  - *cf. Using JavaScript Object: `{1: 1, 2: 3, 3: 2, 4: 1}`.*
- If we continue through the algorithm, the hash table becomes `{2 => 0.42857142857142855, 1 => 0.125, 4 => 0.5, 3 => 0.5, 5 => 0}` for the failure rates of each stage.
  - *cf. Using JavaScript Object: `{1: 0.125, 2: 0.42857142857142855, 3: 0.5, 4: 0.5, 5: 0}`.*
- Therefore, the final `keys` sorted in decreasing order of failure rates will be `[4,3,2,1,5]` since Stage 4 was inserted earlier than Stage 3.
  - *cf. Using JavaScript Object: `[3,4,2,1,5]`.*
- However, one of the constraints mentioned that if two stages have the same failure rates, the earlier stage (Stage 3 in this case) should come before. Using `Map`, as it inserts in insertion order, does not guarantee this.
- To solve this, we could sort `stages` in increasing order at the beginning of the algorithm. However, this results to a slower time complexity (O(n log n)).
