# Big O: Two Sum

## Prompt

Given an array of integers ```nums``` and an integer ```target```, return the indices of the two numbers that add up to ```target```.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order: [1, 3] and [3, 1] are both valid.

## Example 1
```js
Input: nums = [ 2, 7, 11, 15 ], target = 9
Output: [ 0, 1 ]
Explanation: nums[0] = 2 and nums[1] = 7. They both add up to target which is 9.
```
## Example 2
```js
Input: nums = [ 3, 2, 4 ], target = 6
Output: [ 1, 2 ]
Explanation: nums[1] = 2 and nums[2] = 4. They both add up to target which is 6.
```

## Good questions that interviewees might ask: 
1) Is the input sorted? 
The array is not sorted (although you CAN sort it if you want to)

2) Will there always be a valid pair that can add up to target?
Yes, there will be exactly one pair. There will never be 0 valid pairs or more than one valid pair.

## Common mistakes
1) Sorting first, then two-pointer. This will complicate the problem because you need to return the indices. When you sort it, the indices are changed.

## Solution
#### Brute force
Loop through every single pair. If they add up to the target, return the indices.
```js
const twoSum = (nums, target) => {
  // loop through nums
  for(let i = 0; i < nums.length; i++) {
    // loop through nums again, but starting at the current index + 1. this guarantees that i !== j 
    // and you will not go through pairs that you already checked before. example: 
    // [ 1, 2, 3 ]
    //   ^  ^
    //   i  j
    
    // [ 1, 2, 3 ]
    //   ^     ^
    //   i     j
    
    // [ 1, 2, 3 ]
    //      ^  ^
    //      i  j
    
    for(let j = i + 1; j < nums.length; j++) {
      const num1 = nums[i];
      const num2 = nums[j];
      if(num1 + num2 === target) return [ i, j ];
    }
  }
  
  // the pair is not found (this will never happen)
  return [ -1, -1 ]
}
```
Time complexity: O(n^2) N = number of items in the array.
Explanation: you loop through each element N times => O(N * N) = O(N ^ 2).

Space complexity: O(1).
Explanation: no data structures are used.

#### Map
```js
const twoSum = (nums, target) => {
  // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map
  const map = new Map();
  
  for(let i = 0; i < nums.length; i++) {
    const currentNumber = nums[i];
    
    // get the complement of the number. the complement is basically: 
    // currentNumber + COMPLEMENT = target => target - currentNumber = COMPLEMENT
    const complement = target - currentNumber;
    
    // if the map has already found the other number that you need in order to get to target, return both indices
    if(map.has(currentNumber)) {
      return [ map.get(currentNumber), i ];
    }
    
    // put the complement into the map, with the index as the value
    map.set(complement, i);
  }
}

/*

  this algorithm basically says: for each number in the array, 
  calculate the OTHER number that you would need in order to get to target. 
  
  iteration #0                  resulting map. key is the complement, value is the index that the complement is at
  [2,7,11,15] target = 9        {
   ^                              7: 0
   does the map have 2? no:     }
   complement = 9 - 2 = 7   
   
  iteration #1                  resulting map. key is the complement, value is the index that the complement is at
  [2,7,11,15] target = 9        {
     ^                            7: 0
   does the map have 7? yes:    }
   return [ 0(obtained from the map.get(7)), i (the current iteration # we're at) ]
*/
```

Time complexity: O(N), N = number of elements in array.
Explanation: just a single pass through the input array.

Space complexity: O(N), N = number of elements in the array. 
Explanation: 
[ 1, 2, 3, 4 ] target = 7. 
resulting map: 
{
   1: 0,
   2: 1,
   3: 2
}

#### Object instead of map
```js
const twoSum = (nums, target) => {
  const map = {};

  for(let i = 0; i < nums.length; i++) {
    const currentNumber = nums[i];
    
    // get the complement of the number. the complement is basically: 
    // currentNumber + COMPLEMENT = target => target - currentNumber = COMPLEMENT
    const complement = target - currentNumber;
    
    // if the map has already found the other number that you need in order to get to target, return both indices
    if(currentNumber in map) {
      return [ map[currentNumber], i ];
    }
    
    // put the complement into the object, with the index as the value
    map[complement] = i;
  }
}
```
Time and space complexity: same as the solution using map

This is a good time to talk about "trading" space complexity for time complexity. The first solution uses more time, but less space. The second/third solution uses less time but more space. In general, the preferred solution is the one with the best TIME complexity.
