# 2022 KAKAO SUMMER TECH INTERNSHIP

## 성격 유형 검사하기
- **Inputs:**
  - An array of strings `survey` where `survey[i]` represents the category that each question indicates.
    - The first character in `survey[i]` represents the type for the disagree option for the `i+1`<sup>th</sup> question.
    - The second character in `survey[i]` represents the type for the agree option for the `i+1`<sup>th</sup> question.
  - An array of positive integers `choices` where `choices[i]`represents the option that the testtaker chose.
    - `choices[i]` represents the option number of the `i+1`<sup>th</sup> question.
- **Description**
  - Create a personality type test.
  - There are four categories with two types each (therefore, 16 permutations).
    | Category | Types |
    | -------- | ----- |
    | 1        | R, T  |
    | 2        | C, F  |
    | 3        | J, M  |
    | 4        | A, N  |
  - There are `n` number of questions each with 7 options.
    - Each question is dedicated to and give points for a single category (either of the two types for that category).
    - Example
      | Option Number | Option            | Points | Type |
      | ------------- | ----------------- | ------ | ---- |
      | 1             | Strongly Disagree | 3      | R    |
      | 2             | Disagree          | 2      | R    |
      | 3             | Slightly Disagree | 1      | R    |
      | 4             | Not Sure          | -      | -    |
      | 5             | Slightly Agree    | 1      | T    |
      | 6             | Agree             | 2      | T    |
      | 7             | Strongly Agree    | 3      | T    |
      - *The options and the corresponding types depend on the question.*                     
        - i.e. in another question, type `T` can be the disagree option and type `R` can be the agree option.
  - The types with the more points is considered to be the type of the testtaker.
    - *If the two types of a single category share the same number of points, the type is decided based on the alphabet order.*
- **Output:** return a string representing the personality type of the testtaker.
- **Constraints**
  - `1 <= survey.length ( = n) <= 1,000`.
  - `survey[i]` is either one of these: "RT", "TR", "FC", "CF", "MJ", "JM", "AN", "NA".
  - `choices.length === survey.length`.
  - `1 <= choices[i] <= 7`.
### Example
```js
solution(["AN", "CF", "MJ", "RT", "NA"], [5, 3, 2, 7, 5]); // "TCMA"
solution(["TR", "RT", "TR"], [7, 1, 3]); // "RCJA"
```
### Solutions
#### Solution 1
```js
const solution = (survey, choices) => {
  let ans = "";

  const pointsSystem = [3, 2, 1, 0, 1, 2, 3];

  // Initialize tally.
  const types = ["R", "T", "C", "F", "J", "M", "A", "N"];
  const tally = {};
  for (let type of types) {
    tally[type] = 0;
    }

  // Accumulate points.
  for (let i = 0; i < survey.length; i++) {
    const choice = choices[i];

    const points = pointsSystem[choice-1];

    let type;
    if (choice < 4) type = survey[i][0];
    else if (choice > 4) type = survey[i][1];

    tally[type] += points;
  }

  // Determine personality.
  res += tally["R"] >= tally["T"] ? "R" : "T";
  res += tally["C"] >= tally["F"] ? "C" : "F";
  res += tally["J"] >= tally["M"] ? "J" : "M";
  res += tally["A"] >= tally["N"] ? "A" : "N";

  return ans;
}
```
#### Solution 2
```js
const solution = (survey, choices) => {
  const points = [3,2,1,0,1,2,3];

  const pointsPerCategoryType = {
    category1: { R: 0, T: 0 },
    category2: { C: 0, F: 0 },
    category3: { J: 0, M: 0 },
    category4: { A: 0, N: 0 },
  };
  
  const addPoints = (category, type1, type2, choice) => {
    if (choice < 4) pointsPerCategoryType[category][type1] += points[choice - 1];
    else if (choice > 4) pointsPerCategoryType[category][type2] += points[choice - 1];
  }
  
  for (let i = 0; i < survey.length; i++) {    
    const [type1, type2] = survey[i];
  
    if (type1 === "R" || type1 === "T") addPoints("category1", type1, type2, choices[i]);
    else if (type1 === "C" || type1 === "F") addPoints("category2", type1, type2, choices[i]);
    else if (type1 === "J" || type1 === "M") addPoints("category3", type1, type2, choices[i]);
    else if (type1 === "A" || type1 === "N") addPoints("category4", type1, type2, choices[i]);
  }
  
  let ans = "";
  for (let category of Object.values(pointsPerCategoryType)) {
    const type = Object.keys(category).reduce((a,b) => result[a] >= result[b] ? a : b, "");
    ans += type;
  }
  return ans;
}
```

## Make Two Queues Sum Equal
- **Inputs**
  - A queue `queue1` represented in an array.
  - A queue `queue2` represented in an array.
- **Description**
  - A single operation consists of popping an element from one queue and pushing it to the other queue.
- **Output:** return the number of operations needed to make the queues have the same sum.
### Example
```js
solution([3, 2, 7, 2], [4, 6, 5, 1]);
solution([1, 2, 1, 2], [1, 10, 1, 2]);
solution([1, 1], [1, 5]);
```
### Solution
- Add the sum of both queues and divide it by 2 to get the target sum.
- Concatenate the two queues into one array.
- Use two pointers to find the point where the target sum is achieved.
  - First pointer initialized to index 0.
  - Second pointer initialized to the start of the second queue.
```js
const solution = (queue1, queue2) => {
  const targetSum = queue1.reduce((sum,x) => sum + x, 0) + queue2.reduce((sum,x) => sum + x, 0);
  const arr = [...queue1, ...queue2];
  let leftPointer = 0;
  let rightPointer = queue1.length;
  while (leftPointer <= rightPointer && rightPointer < arr.length) {
    const runningSum = arr.slice(leftPointer, rightPointer+1).reduce((sum, x) => sum + x, 0);
    if (runningSum < targetSum) rightPointer++;
    else if (runningSum > targetSum) leftPointer++;
    else return leftPointer + (rightPointer - (queue1.length - 1));
  }
  return -1;
}
```
