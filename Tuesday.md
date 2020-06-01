# Nth Fibonacci Number

## Prompt
Write a function that takes a positive integer n and returns the nth number in the Fibonacci sequence.

The Fibonacci sequence starts with the numbers 0 and 1. Every subsequent number is the sum of the two numbers that preceed it. For example, the first 10 Fibonacci numbers are:
```
Number:      1   2   3   4   5   6   7   8   9   10  etc.
Fib Number:  0,  1,  1,  2,  3,  5,  8,  13, 21, 34, etc.
```

## Examples
```javascript
fib(1)  // returns 0
fib(3)  // returns 1
fib(5)  // returns 3
fib(8)  // returns 13
fib(64) // returns 6557470319842
```

## Interviewer Strategy Guide
- If your interviewee struggles with an approach, remind them that a Fibonacci number is calculated by the formula `fib(n) = fib(n-1) + fib(n-2)`. Can they use recursion here? What is the base case? Recursive case?

- If they don't know how to calculate the time complexity of Solution 1:
  - A GREAT shorthand for calculating the number of function calls in a recursive function is `numBranches^depth`
  - For example, a binary tree with depth n has a runtime is O(2^n)

- If they reach the unoptimized recursive solution, ask them to take a look at their function calls and notice if there is any repetition (where there is repetition, there is room for optimization!)

- It may help the interviewee notice repetition if they draw out function calls and see that the whole second recursive term `fib(n-2)` should already be calculated from the first term, `fib(n-1)` (see the tree below)

- If they reach the partially optimized solution with a hash table, ask them if they can optimize further
  - They may need to rewrite their solution iteratively to notice that we do not need a hash table with every fib number calculated
  - We really only need the last two numbers at any point in time to continue with our calculations.

**Visualizing Recursive Calls for Fibonacci**
```
        Solution 1: No Memoization                   Solution 2: With Memoization

                 fib(n)                                          fib(n)
               /        \                                      /        \
        fib(n-1)        fib(n-2)                        fib(n-1)        in hash
        /      \        /      \                        /      \
  fib(n-2)  fib(n-3)  fib(n-3)  fib(n-4)          fib(n-2)    in hash


            ...continued until fib is called with zeroes and ones
```

## Solutions

### Solution 1: Recursion without Memoization (Unoptimized)

**Strategy:** Use recursive calls to calculate the fibonacci number, using the fact that the fib term for any given number n is `fib(n-1) + fib(n-2)`.

**Time Complexity:** O(2^n). As stated above, you can calculate the number of function calls in a recursive function as `numBranches^depth`. At each recursive function call, we only perform constant time operations so the overall time complexity is O(2^n).

**Space Complexity:** O(n). Each branch of the initial recursive function call will add to the callstack until it is n high. So the space taken is O(n).

```javascript
function fib(num) {
  // Fancy way of writing the base case: if num is 1 return 0 and if num is 2 return 1
  if (num < 2) return num - 1;

  //Recursive Case
  else return fib(num - 1) + fib(num - 2);
}
```

### Solution 2: Recursion with Memoization (Partially Optimized)
**Strategy:** Use recursive calls to calulate the fibonacci number, using memoization (i.e. add to a hash table) to store the result before returning.

**Time Complexity:** O(n). Using a hash table lets us eliminate one whole branch of calculations at every recursive call (see the diagram above). Now, we only make n function calls overall.

**Space Complexity:** O(n). The callstack is n calls high before beginning to resolve.

```javascript
const initialHash = {
  //num: fib(num)
  1: 0,
  2: 1
}

function fib(num, hash = initialHash) {
  // If the hash table doesn't contain this num yet, calculate it
  if (hash[num] === undefined) {
    //The hash table is passed by reference so we are able to add to it at any function call in the stack
    hash[num] = fib(num - 1, hash) + fib(num - 2, hash);
  }

  return hash[num];
}
```

### Solution 3: Iteration (Optimized)
**Strategy:** Initialize a counter variable to represent the current fib number that we can calulate, and an array to hold the last two numbers of the Fibonacci sequence in relation to a counter. Iterate the counter up to the input number, updating the array as you go to hold the *new* last two numbers of the sequence.

**Time Complexity:** O(n). We perform constant-time calculations for every number up to the input number. This give us O(n).

**Space Complexity:** O(1), or constant, space. Although we do initialize an array, this array only ever holds 2 elements. This space is trivial.

```javascript
function fib(num) {
  // Fancy way of writing the base case: if num is 1 return 0 and if num is 2 return 1
  if (num < 2) return num - 1;

  // Inialize an array to hold the last two numbers in the fib sequence
  let lastTwoNums = [0, 1];
  // Counter represents the nth fib number that is currently the sum of the lastTwoNums array
  let counter = 3

  while (counter < num) {
    // Reassign the last two numbers in the array
    const temp = lastTwoNums[0];
    lastTwoNums[0] = lastTwoNums[1];
    lastTwoNums[1] = lastTwoNums[1] + temp;

    // You could use this line to replace the above 3 lines of code
    // lastTwoNums = [lastTwoNums[1], lastTwoNums[0]+lastTwonums[1]]

    counter++;
  }

  return lastTwoNums[0] + lastTwoNums[1];
}
```
