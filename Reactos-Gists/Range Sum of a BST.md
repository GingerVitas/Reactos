# Range Sum of a BST

## Prompt
Given a binary search tree and two integers `low` and `high`, return the sum of all nodes whose values are in the **inclusive** range [low, high]. 

## Example
![img](https://miro.medium.com/max/712/1*4M5MU3CqJYGNExEi5Ttuew.png)
```js
Test Input:

class Tree {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

const tree = new Tree(7);
tree.left = new Tree(2);
tree.right = new Tree(9);
tree.left.left = new Tree(1);
tree.left.right = new Tree(5);
tree.right.right = new Tree(14);

Low = 7
High = 14
```
Output: 30  
Nodes 7, 9 and 14 are in the range of [7, 14] (inclusive). 7 + 9 + 14 = 30  

## Hints
1. How would you check every single value in a tree?
2. BFS or DFS (you should probably know these two algos like the back of your hand LOL because they're used in SOOOOOOOO many interviews)

## Solutions
Well lets just start with the general BFS and DFS algorithm

### BFS GENERAL ALGORITHM
1. make a queue with your starting point
2. while (queue.length) ...
3. take the first item out of the queue
4. process the item
5. push item's children into the queue
```js
const queue = [root];
while (queue.length) {
  const node = queue.shift(); // this is an O(N) operation but during the interview,
                              // just let them know that you're using an array as a queue for simplicity
                              // and that in the real world, you'd use an actual queue which is O(1)
  
  // do whatever you want with the node's value
  console.log(node.value);
  
  // push the children into the queue
  if (node.left) queue.push(node.left);
  if (node.right) queue.push(node.right);
}
```
### DFS General Algorithm
1. base case: if your node is null, just return
2. process the node
3. call dfs with the node's children   
  a. `dfs(node.left); dfs(node.right);`  
```js
if (!node) return;

// do whatever you want with the node
console.log(node.value);

dfs(node.left);
dfs(node.right);
```

DFS is O(N) time  
O(HEIGHT) space  

### Solution 1- DFS
1. if root doesnt exist, return 0
2. if our root's value is in the inclusive range of [low, high], add it to our sum. if not, dont add anything to the sum
3. traverse the left and right children
```js
const rangeSumBST = function(root, low, high) {
  // base case
  if (!root) return 0;
  
  // process the node
  let value = 0;
  if (root.val >= low && root.val <= high) value = root.val;
  
  // traverse the children
  return value + rangeSumBST(root.left, low, high) + rangeSumBST(root.right, low, high);
};
```
Time: O(N)  
Space: O(HEIGHT) because of recursion stack space   

### Solution 1.5: DFS but simpler
```js
const sumWithinRange = (startingPoint, low, high) => {
    let sum = 0;

    const recursion = (node) => {
        if (!node) return 0;

        if (node.value >= low && node.value <= high) {
            sum += node.value;
        }

        recursion(node.left);
        recursion(node.right);
    }
    
    recursion(startingPoint);
    return sum;
};
```

### Solution 2- BFS
1. initialize our sum at 0
2. initialize BFS. at every single node, check whether it's in the inclusive range [low, high]
3. if it is, add it to our sum
```js
const rangeSumBST = function(root, low, high) {
    let sum = 0;
    
    // 1. make a queue with our starting point
    const queue = [root];
    
    // 2. while queue.length...
    while(queue.length) {
        // 3. take the node out of the queue
        const node = queue.shift();
        
        // 4. process the node
        if(node.val >= low && node.val <= high) {
            sum += node.val;
        }
        
        // 5. traverse the children
        if(node.left) queue.push(node.left);
        if(node.right) queue.push(node.right);
    }
    
    return sum;
};
```
Time: O(N)   
Space: O(N) because of the queue   
