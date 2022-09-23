# Tree

## 1. [Same Tree](https://leetcode.com/problems/same-tree/)
- DFS both trees at the same time and compare structure and value.
```js
// TC: O(n), SC: O(1)

const isSameTree = (p, q) => {
  if (p === null && q === null) return true;
  if (p === null || q === null) return false;
  if (p.val !== q.val) return false;
  
  return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

## 2. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
### Solution 1 - DFS
```js
// TC: O(n), SC: O(n)

const maxDepth = (root) => {
  if (root === null) return 0;
  
  const leftDepth = maxDepth(root.left);
  const rightDepth = maxDepth(root.right);
  return 1 + Math.max(leftDepth, rightDepth);
}
```
### Solution 2 - BFS
```js
// TC: O(n), SC: O(n)

const maxDepth = (root) => {
  let depth = 0;
  
  // level represents all the nodes in the current level.
  let level = root ? [root] : [];
  while (level.length > 0) {
    depth++;
    
    // Gather the nodes for the next level.
    const nextLevel = [];
    for (let node of level) {
      if (node.left) nextLevel.push(node.left);
      if (node.right) nextLevel.push(node.right);
    }
    
    level = nextLevel;
  }
  
  return depth;
}
```

## 3. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
### Solution 1 - DFS Postorder
- Use DFS Postorder to traverse the leftmost node, then the rightmost node, then the node.
```js
// TC: O(n), SC: O(n)

const invertTree = (root) => {
  const dfs = (node) => {
    if (node === null) return;
    
    if (node.left) dfs(node.left);
    if (node.right) dfs(node.right);
    
    [node.left, node.right] = [node.right, node.left];
  }
  
  dfs(root);
  
  return root;
}
```
### Solution 2 - BFS
```js
// TC: O(n), SC: O(n)

const invertTree = (root) => {
  if (root === null) return null;
  
  const queue = [root];
  let currNode;
  while (queue.length > 0) {
    currNode = queue.shift();
    [currNode.left, currNode.right] = [currNode.right, currNode.left];
    
    if (currNode.left) queue.push(currNode.left);
    if (currNode.right) queue.push(currNode.right);
  }
  
  return root;
}
```

## 4. [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)
- Traverse through `root` and if the current node's value is the same as `subRoot`'s (head's) value.
```js
// TC: O(n*m), SC: O(1)
  // n: the number of nodes in root.
  // m: the number of nodes in subRoot.

const isSubtree = (root, subRoot) => {
  // If the nodes are the same, recursive call will keep going until the end (both nodes are null).
  const isSame = (treeNode, subtreeNode) => {
    if (treeNode === null && subtreeNode === null) return true;
    if (treeNode === null || subtreeNode === null) return false;
    if (treeNode.val !== subtreeNode.val) return false;
    return isSame(treeNode.left, subtreeNode.right) && isSame(treeNode.right, subtreeNode.right);
  }
  
  // Return true if subtree exists, false if not.
  // The `||` guarantees true if a subtree has been found anywhere in the tree.
  const dfs = (node) => {
    if (node === null) return false;
    if (node.val === subRoot.val && isSame(node, subRoot)) return true;
    return dfs(node.left) || dfs(node.right);
  }
  
  return dfs(root);
}
```

## 5. [Lowest Common Ancestor of BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)
- The `root` is guaranteed to be a common ancestor so start loop from there.
- If the current node is either `p` or `q`, return current node.
- If `p` or `q` split up from current node, return current node.
```js
// TC: O(log n), SC: O(1)

const lowestCommonAncestor = (root, p, q) => {
  let currNode = root;
  while (true) {
    if (p.val < root.val && q.val < root.val) currNode = currNode.left;
    else if (p.val > root.val && q.val > root.val) currNode = currNode.right;
    else return currNode;
  }
}
```

## 6. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- Use BFS.
  - At the start of an iteration, the nodes in `queue` represents all the nodes in the current level.
  - However, the next level's nodes are added to `queue` in the process.
```js
// TC: O(n), SC: O(n)

const levelOrder = (root) => {
  const ans = [];
  
  const queue = [root];
  
  while (queue.length > 0) {
    const currLevel = [];
    const numNodesInCurrLevel = queue.length;
    
    for (let i = 0; i < numNodesInCurrLevel; i++) {
      const currNode = queue.shift();
      
      if (currNode !== null) {
        currLevel.push(currNode.val);
        queue.push(currNode.left, currNode.right);
      }
    }
    
    if (currLevel.length > 0) ans.push(currLevel);
  }
  
  return ans;
}
```

## 7. [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
- `preorder[0]` is always the root of the tree.
- The nodes before `preorder[0]` in `inorder` are on the left of the root.
- The nodes after `preorder[0]` in `inorder` are on the right of the root.
```js
// TC: O(n), SC: O(n)

const buildTree = (preorder, inorder) => {
  if (preorder.length === 0 || inorder.length === 0) return null;
  
  const root = preorder[0];
  const rootPos = new TreeNode(preorder[0]);
  
  root.left = buildTree(preorder.slice(1, rootPos + 1), inorder.slice(0, rootPos + 1));
  root.right = buildTree(preorder.slice(rootPos + 1), inorder.slice(rootPos + 1));
  
  return root;
}
```

## 8. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
- Valid BST condition for each node.
  - `node.left < node < node.right`.
  - If node is a "left" child:
    - `-Infinity < node < parentNode`.
  - If node is a "right" child:
    - `parentNode < node < Infinity`.
```js
// TC: O(n), SC: O(n)

const isValidBST = (root) => {
  const dfs = (node, leftBound, rightBound) => {
    if (node === null) return true;
    if (!(leftBound < node.val && node.val < rightBound)) return false;
    
    return dfs(node.left, leftBound, node.val) && dfs(node.right, node.val, rightBound);
  }
  
  return dfs(root, -Infinity, Infinity);
}
```

## 9. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
### Solution 1 - Recursive Inorder Traversal
- Collect the first `k` values through inorder traversal.
```js
// TC: O(k+1), SC: O(k+1)

const kthSmallest = (root, k) => {
  const values = [];
  
  const dfsInorder = (node) => {
    if (values.length < k) {
      if (node.left) dfsInorder(node.left);
      values.push(node.val);
      if (node.right) dfsInorder(node.right);
    }
  }
  
  dfsInorder(root);
  
  return values[k-1];
}
```
### Solution 2 - Iterative Inorder Traversal
```js
// TC: O(k), SC: O(k)

const kthSmallest = (root, k) => {
  const stack = [];
  let currNode = root;
  
  while (currNode !== null || stack.length > 0) {
    // Traverse to the leftmost node.
    while (currNode !== null) {
      stack.push(currNode);
      curNode = currNode.left;
    }
    // Once reached the leftmost node, pop from stack (to get the smallest number).
    currNode = stack.pop();
    // Check k.
    if (--k === 0) return currNode.val;
    // Move on to the right node.
    currNode = currNode.right;
  }
}
```

## 10. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)
### Solution 1 - Class-based
- A TrieNode should contain two information: its children in an object, and if it is an end of a word.
```js
class TrieNode {
  constructor() {
    this.children = {};
    this.endOfWord = false;
  }
}

class Trie {
  constructor() {
    this.root = null;
  }
  
  insert(word) {
    let currNode = this.root;
    for (let char of word) {
      // If a node for the character doesn't exist, create one.
      if (!(char in currNode.children)) currNode.children[char] = new TrieNode();
      // Move currNode.
      currNode = currNode.children[char];
    }
    // Record the letter to indicate the end.
    currNode.endOfWord = true;
    return;
  }
  
  search(word) {
    let currNode = this.root;
    for (let char of word) {
      if (!(char in currNode.children)) return false;
      currNode = currNode.children[char];
    }
    return currNode.endOfWord;
  }
  
  startsWith(prefix) {
    let currNode = this.root;
    for (let char of word) {
      if (!(char in currNode.children)) return false;
      currNode = currNode.children[char];
    }
    return true;
  }
}
```
### Solution 2 - Function Prototype-based
- A trie node contains its children as key-value pairs where the key is the child's alphabet and the value is the node object, and if it is an end of a word.
```js
const Trie = function() {
  this.root = {}; // a node is represented as an object.
}

Trie.prototype.insert = function(word) {
  let currNode = this.root;
  for (let char of word) {
    if (!currNode[char]) currNode[char] = {};
    currNode = currNode[char];
  }
  currNode.endOfWord = true;
  return;
}

Trie.prototype.search = function(word) {
  let currNode = this.root;
  for (let char of word) {
    if (!currNode[char]) return false;
    currNode = currNode[char];
  }
  return currNode.endOfWord === true;
}

Trie.prototype.startsWith = function(prefix) {
  let currNode = this.root;
  for (let char of prefix) {
    if (!currNode[char]) return false;
    currNode = currNode[char];
  }
  return true;
}
```

## 11. [Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)
- Implement a Trie structure.
- For `search`, use DFS to handle `"."`s.
```js
class TrieNode {
  constructor() {
    this.children = {};
    this.endOfWord = false;
  }
}

class WordDictionary {
  constructor() {
    this.root = new TrieNode();
  }
  
  addWord(newWord) {
    let currNode = this.root;
    for (let char of newWord) {
      if (!(char in currNode.children)) currNode.children[char] = new TrieNode();
      currNode = currNode.children[char];
    }
    currNode.endOfWord = true;
    return;
  }
  
  search(word) {
    const dfs = (node, idx) => {
      for (let i = idx; i < word.length; i++) {
        // If encountered a ".", call dfs on the current node's children.
        if (word[i] === ".") {
          for (let child of Object.values(node.children)) {
            if (dfs(child, i + 1) === true) return true;
          }
          return false;
        } 
        // Otherwise (alphabet), normal traverse.
        else {
          if (!(word[i] in node.children)) return false;
          node = node.children[word[i]];
        }
      }
      return node.endOfWord;
    }
    
    return dfs(this.root, 0);
  }
}
```
