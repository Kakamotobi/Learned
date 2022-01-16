# Binary Search Trees - Medium

## 1382. Balance a Binary Search Tree
- Input: `root` of a BST.
- Output: return a balanced binary search tree with the same node values.
- Constraints
  - Number of nodes range between `[1, 10^4]`.
  - `1 <= Node.val <= 10^5`.
### Solution - inorder DFS and binary search
- Record the node values in sorted order.
- Use binary search on the sorted array to construct a balanced BST.
```js
const balanceBST = (root) => {
  const values = [];
  function inorder(node) {
    if (node.left) inorder(node.left);
    values.push(node.val);
    if (node.right) inorder(node.right);
  }
  inorder(root);
  
  // Ultimately returns the root.
  function constructBST(values, start=0, end=values.length-1) {
    if (start > end) return null;
    const mid = Math.floor((start+end)/2);
    const currNode = new TreeNode(values[mid]);
    currNode.left = constructBST(values, start, mid-1);
    currNode.right = constructBST(values, mid+1, end);
    return currNode;
  }
  
  return constructBST(values);
}
```
