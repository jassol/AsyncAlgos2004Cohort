# Branch Sums

## Prompt
Write a function that takes a binary tree and returns an array of its branch sums ordered from leftmost branch sum to rightmost branch sum. A branch sum is the sum of all the values in a Binary Tree branch, extending from the root node to a leaf node.

As a refresher, a node in a binary tree is an object with a `value`, a `left` child, and a `right` child.

## Examples
```
Input:
         1

Output: [1]
```
```
Input:
         1
       /  \
      2    3

Output: [3, 4]
```
```
Input:
         1
       /  \
      2    3
     /    / \
    4    5   6

Output: [7, 9, 10]
```
```
Input:
         1
       /  \
      2    3
     /    / \
    4    5   6
   / \      /
  7   8    9

Output: [14, 15, 9, 19]
```

## Interviewer Strategy Guide

- If your interviewee struggles with an initial approach:
  - We need to visit at each node in the tree at least once, to add the each node's value to the branch sum
  - What ways do we know to traverse a tree?
  - Does breadth-first search (BFS) or depth-first search (DFS) make more sense here?

- If they identify DFS as a solid approach:
  - What type of DFS makes sense if you want to process the root node first, and then recurse onto the nodes children? (pre-order!!)
  - Notice that there is a branch sum in the return array for every leaf node in the tree.
  - How do we want to process a node? What do we do if it is a leaf node? What do we do if it has children?

- If they struggle with summing the branch, or adding to the return array:
  - How can we pass information about past node values between different function calls on the callstack?
  - What if we passed the return array to the function as a parameter? Then we could add branch sums whenever we want
  - Similarly, what if passed the running total to the function as a parameter? How could we add to this running total?

## Solutions

### Solution 1: Recursion (Optimized)
**Strategy:** Use pre-order DFS (process the root and then process the children) to traverse the tree. Use a `sums` array to hold final sums for each branch (we can add to the `sums` array from any call on the callstack because arrays are PBR). Use a `runningTotal` counter to add the nodes as the traverse the branch (the counter for each branch is separate, because numbers are PBV). Add the running total to the return array when you reach a lead node.

**Time Complexity:** O(n). We traverse the tree and visit every node once.

**Space Complexity:** O(n). We can imagine the most unbalanced tree possible, where every node only has a right child. In this situation, the callstack would have `n` nodes in is before beginning to resolve. So this is O(n) space.

```javascript
function branchSums(node, sums = [], runningTotal = 0) {

  // Process the node: add its value to our running total for this branch
	runningTotal = runningTotal + node.value;

	// Base Case: we are at a leaf node (both children are null)
  // Our branch sum is complete so we can add it to our return array
	if (!node.right && !node.left) sums.push(runningTotal);

	// Recursive Case: we are at node that has one or more children
  // Call the function on the child nodes with the new runningTotal
	else {
		if (node.left) branchSums(node.left, sums, runningTotal);
		if (node.right) branchSums(node.right, sums, runningTotal);
	}

	// This return statement is only important for the first function call
	return sums;
}
```
