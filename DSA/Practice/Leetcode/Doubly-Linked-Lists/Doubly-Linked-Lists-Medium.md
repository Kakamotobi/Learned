# Doubly Linked Lists - Medium

## 1472. Design Browser History
- Description
  - You have a browser of one tab starting at the `homepage`.
  - You can visit another `url`, go back/forward in the history number of `steps`.
- Instructions
  - `BrowserHistory(homepage)` class initializes the object with the `homepage`.
  - `BrowserHistory.prototype.visit(url)` visits `url` from the current page, clearing up all the forward history.
  - `BrowserHistory.prototype.back(steps)` moves back `steps` back in history. Return the current `url` after moving back at most steps.
  - `BrowserHistory.prototype.forward(steps)` moves forward `steps` forward in history. Return the current `url` after moving forward at most steps. 
### Example
```js
// Input:
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]
// Output:
[null,null,null,null,"facebook.com","google.com","facebook.com",null,"linkedin.com","google.com","leetcode.com"]
```
### Solution
```js
const BrowserHistory = (homepage) => {
  this.page = {
    url: homepage,
    prev: null,
    next: null,
  };
};

BrowserHistory.prototype.visit = (url) => {
  this.page.next = {
    url,
    prev: this.page,
    next: null,
  };
};

BrowserHistory.prototype.back = (steps) => {
  while (this.page.prev && steps) {
    this.page = this.page.prev;
  };
  return this.page.url;
};

BrowserHistory.prototype.forward = (steps) => {
  while (this.page.next && steps) {
    this.page = this.page.next;
  }
  return this.page.url;
}
```
