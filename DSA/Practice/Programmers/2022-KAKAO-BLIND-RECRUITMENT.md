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
- Use the above hashtable to construct another hashtable representing the number of emails that a reporter should receive.
- Use the above hashtable and `id_list` to return the number of emails each user should receive.
```js
const solution = (id_list, report, k) => {
  const whoReportedMe = {};
  const numEmails = {};
  
  // Construct whoReportedMe.
  for (let r of report) {
    const users = r.split(" ");
    
    whoReportedMe[users[1]] = whoReportedMe[users[1]] !== undefined ? whoReportedMe[users[1]].add(users[0]) : new Set([users[0]]);
    // or ES2020+
    // whoReportedMe[users[1]] = whoReportedMe[users[1]]?.add(users[0]) || new Set([users[0]]);
  }
  
  // Construct numEmails.
  for (let key in whoReportedMe) {
    if (whoReportedMe[key].size >= k) {
      for (let reporter of whoReportedMe[key].values()) {
        numEmails[reporter] = ++numEmails[reporter] || 1;
      }
    }
  }
  
  // Compute results.
  for (let i = 0; i < id_list.length; i++) {
    id_list[i] = id_list[i] in numEmails ? numEmails[id_list[i]] : 0;
  }
  
  return id_list;
}
