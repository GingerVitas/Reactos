# Fibonacci Numbers

## Prompt
Given a number N, find the Nth fibonacci number.  
Fibonacci numbers are "the sum of the two previous fibonacci numbers". The first fib number is `0`, the second fib number is `1` (These are hardcoded)  

## Example
`fib(1)` -> 0  
`fib(2)` -> 1  
`fib(3)` -> 1  
`fib(4)` -> 2 (because fib(2) is 1, fib(3) is 1. 1 + 1 is 2)  
`fib(5)` -> 3 (1 + 2)  
`fib(6)` -> 5 (2 + 3)  
`fib(7)` -> 8 (5 + 5)  
`fib(8)` -> 13  

## Solution 1: Brute force recursive
```js
const fib = (num) => {
  if (num === 1) return 0;
  if (num === 2) return 1;
  
  return fib(num - 1) + fib(num - 2);
}
```
Time complexity: O(2^N)    
Space complexity: O(N) recursion stack

## ONCE THEY INEVITABLY SOLVE IT- ask them what the time complexity is. Try it with a "big" number like.. 40. Notice how slow it is! Ask them to improve the time

## Hints
1. Draw out the recursive tree for this on excalidraw  
It should look like this  
![img](https://i.stack.imgur.com/7iU1j.png)
2. After drawing out the recursive tree, point out how much duplicate work we're doing! We're re-calculating `fib(3)` and `fib(2)` many many times even though we know we're getting the same answer every time. How can we "remember" the answers?
3. Cache the answers

## Solution 2: Recursive with *MEMOIZATION*. Also known as "Top-down dynamic programming"
`Memoization` means you're "saving" results that you have calculated inside a `memo`, hence `memoization`  
```js
const fib = (num, cache = {}) => {
  if (num in cache) return cache[num];
  if (num === 1) return 0;
  if (num === 2) return 1;
  
  const sum = fib(num - 1, cache) + fib(num - 2, cache);
  cache[num] = sum;
  return sum;
}
```
Time complexity: O(N)  
Space: O(N) (recursion tree space + the stuff you're saving in the memo)

## If they get that solution^, ask them "how do we improve on the space?"
## Hints
1. What's the bottleneck for the O(N) space that we're using? It's the recursion stack space. How can we do this iteratively?
2. When we do it iteratively, notice how we only need the PREVIOUS TWO numbers. That means we only need to save the previous two numbers.

## Solution: Iterative solution. Also known as "Bottom-up dynamic programming"
```js
const fib = (num) => {
  // this is fib(n - 2);
  let twoPrevious = 0;

  // this is fib(n - 1);
  let onePrevious = 1;

  for(let i = 3; i <= num; i++) {
  // this is where we calculate fib(n - 1) + fib(n - 2);
    const currentNumber = onePrevious + twoPrevious;

    // lets say twoPrevious is 0
    //          onePrevious is 1
    // that means `current` is 1
    // when we want to calculate the NEXT fib number, 0 is unused
    // this is because we want to calculate 1 + 1, so we can just get rid of `0`
    twoPrevious = onePrevious;
    onePrevious = currentNumber;
  }

  return num > 1 ? onePrevious : twoPrevious;
}
```
Time complexity: O(N)  
Space: O(1)

# Here's a gist I made that times all of these https://replit.com/@StannieLim/KnownLimpingPatches#index.js
