# Find Closest Value BST
Write a function that takes in a Binary Search Tree(BST) and a target integer value and returns the closest value to that target value contained in the BST. You can assume there will only be one closest value.

# Examples
Sample Input: 
```
tree = 10
      /  \
     5    15
    / \   / \
   2   5 13  22
  /        \
 1          14

target = 12
```

Sample Output:  
`13`

# Hints
1. You will need to compare absolute values to determine if a node's value is closer or not. 13 is closer to 12 than 14 but without absolute value negative numbers will cause problems.
2. Remember you can traverse a BST by comparing values. Compare the target to the value and that tells you where to move next.
3. We will probably need a helper function to get through this one

# The General Idea
Step 1:  We have a tree and a target. The tree is made of nodes and we need to find the closest to the target. There will only be on closest value. So if our target is 12 the tree will not have 11 and 13 at the same time. 
Step 2: The prompt wants us to return the closest value not how close. 

# Solution 1 - Recursive
```js
function findClosestValueInBst(tree, target) {
  return findClosestValueHelper(tree, target, tree.value)
}

function findClosestValueHelper(tree, target, closest){
if(tree === null) return closest
if(Math.abs(target - closest) > Math.abs(target - tree.value)){
  closest = tree.value
 }
 if(target < tree.value) {
  return findClosestValueHelper(tree.left, target, closest)
 } else if(target > tree.value){
  return findClosestValueHelper(tree.right, target, closest)
 } else {
  return closest
 }
}
```
Time complexity: O(log(n)) - We are only checking part of the tree depending on the target so as the tree gets larger it has a smaller impact on our time.       
Space complexity: O(log(n)) - Since this is recursive and we have to keep calling the helper function it takes up a little more space.

# Solution 2 - Iterative
```js
function findClosestValueInBst(tree, target) {
  return findClosestValueHelper(tree, target, tree.value)
}

function findClosestValueHelper(tree, target, closest){
 let currentNode = tree
 while (currentNode !== null) {
  if(Math.abs(target - closest) > Math.abs(target - currentNode.value)) {
   closest = currentNode.value
  }
  if(target < currentNode.value){
   currentNode = currentNode.left
  } else if(target > currentNode.value){
   currentNode = currentNode.right
  } else {
   break
  }
 }
 return closest
}
```
Time complexity: O(log(n)) - As above we are only checking part of the tree.    
Space complexity: O(1) - Since we are not recalling our helper function every time we are not using any extra space.
