# Pair Sum

## Prompt
Given an array of numbers sorted in ascending order and a target number, write a function that determines if any 2 numbers in the array add up to the target number. Return true if any 2 different numbers within the array add up to sum. Return false if no 2 numbers in the array add up to sum.

## Examples
```javascript
pairSum([6,8,13], 14)     //returns true

pairSum([3,5,5,9,13], 10) //returns true

pairSum([1,4,7,14], 14)   //returns false
```

## Interviewer Strategy Guide

- If the interviewer asks for clarification on the input array: Numbers are given in ascending order (lowest to highest). A number may appear more than once in the array. The numbers are integers, and can be negative or positive.
- The interviewee's approach may mistakenly add a number to itself to reach the target. This can happen if they are using nested for-loops without the correct boundaries.
- If the interviewee is attempting Solution 1 (nested for-loops), encourage them to think of the micro-optimizations listed in the strategy for Solution 1.

## Solutions

### Solution 1: Nested For-Loops (Unoptimized)

**Strategy:** Using nested for-loops, iterate through each element in the array. For each element, check it against every other element in the array. This is the "naive" or "brute force" solution.
Some micro optimizations:
- Start the inner for loop after the index of the outer for loop (i.e. initialize the inner for loop with `j=i+1` rather than `j=0`).
- Instead of using a boolean flag and returning at the end of the function, you can return early if a match is found.

**Time Complexity:**  O(n^2). The first loop loops at every element in the array, and for each of those elements, the second loop compares it to the other elements. This makes the runtime n^2.

**Space Complexity:**  O(1). No auxiliary data structures are created.

```javascript
function pairSum1(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i+1; j < arr.length; j++) {
      if (arr[j] + arr[i] === target) return true;
    }
  }
  return false;
}
```

### Solution 2: Binary Search (Partially Optimized)
**Strategy:** Iterate through the array. At each element, calulate the partner number that must be in the array for the two to sum to the target. Use a binary search to look through the array for that partner number.

**Time Complexity:** O(n log n). Iterating through every element in the array is n time, and at each element we are performing a binary search which is log n time. This gives us O(n log n) overall.

**Space Complexity:** O(1). No auxiliary data structures are created.

```javascript
function pairSum(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    let partner = target - arr[i];
    if (binarySearch(arr, i, partner)) return true;
  }
  return false;
}

function binarySearch(arr, i, partner) {
  let lP = 0;
  let rP = arr.length - 1;

  while (lP <= rP) {
    //Calulate midpoint and check if it is the partner number
    let mid = Math.floor((lP+rP)/2);

    if (arr[mid] === partner) {
      //If the partner is found at index i, check its neighbors for another copy
      if (mid === i) return arr[mid-1] === partner || arr[mid+1] === partner;
      //Otherwise simply return true
      return true;
    }

    //Increment the pointers
    if (arr[mid] < partner) lP = mid + 1;
    else rP = mid - 1;
  }

  return false;
}
```

### Solution 3: Pointers (Optimized)
**Strategy:** Use pointers that start pointing to the first and last element of the array. Calculate the total. If the target is lower than the current sum, we know that the rightmost number will not be a part of the potential pair of numbers that add to the target. So we can incremenet that pointer to the left. Similarly, we know that if the target is greater than the sum, we can increment the left pointer to the right. We will iterate through the array in this way until the pointers meet.

**Time Complexity:** O(n). We traverse each element in the array only once as the pointers increment towards each other.

**Space Complexity:** O(1). No auxiliary data structures are created.

```javascript
function pairSum(arr, target) {
  let lP = 0;
  let rP = arr.length -1;

  while (lP < rP) {
    //Check the total for the elements at the current pointer positions
    let currentSum = arr[lP] + arr[rP];
    if (currentSum === target) return true;

    //Increment the pointers as needed
    currentSum < target ? lP++ : rP--;
  }

  return false;
}
```
