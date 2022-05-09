# 2022 KAKAO BLIND RECRUITMENT

## [신고 결과 받기](https://programmers.co.kr/learn/courses/30/lessons/92334)
- **Inputs**
  - An array of strings `id_list`, representing a list of user IDs.
  - An array of strings `report`, representing which user reported which user.
    - For each string, `"reporter reported"`.
  - A positive integer `k`, representing the number of reports required for a user to be blocked.
- **Output:** return an array consisting of the number of emails that each user has received (in the same order as `id_list`).
- **Rules**
  - Each user can report a single user per report.
    - There is no limit to the number of reports that can be made.
    - Multiple reports on the same user can be made by the same user. But it will only count as 1.
  - A user that has been reported `k` times is blocked. And every user that has reported that user should receive an email noticing this.
### Example
```js
solution(["muzi", "frodo", "apeach", "neo"], ["muzi frodo","apeach frodo","frodo neo","muzi neo","apeach muzi"], 2); // [2,1,1,0]
solution(["con", "ryan"], ["ryan con", "ryan con", "ryan con", "ryan con"], 3); // [0,0]
```
### Solution
- Construct a hashtable where the key is those that have been reported and the value is a Set of the reporters.
- Loop through the hashtable and find the index of the reporters that should receive emails and accumulate the number of emails each user should receive.
```js
const solution = (id_list, report, k) => {
  const whoReportedMe = {};
  for (let r of report) {
    const users = r.split(" ");
    
    whoReportedMe[users[1]] = whoReportedMe[users[1]] !== undefined ? whoReportedMe[users[1]].add(users[0]) : new Set([users[0]]);
    // or ES2020+
    // whoReportedMe[users[1]] = whoReportedMe[users[1]]?.add(users[0]) || new Set([users[0]]);
  }
  
  const numEmails = new Array(id_list.length).fill(0);
  for (let [reported, reporters] of Object.entries(whoReportedMe)) {
    if (reporters.size >= k) {
      for (let reporter of reporters) {
        const idx = id_list.indexOf(reporter);
        numEmails[idx]++;
      }
    }
  }
  return numEmails;
}
```
