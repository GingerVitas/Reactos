# Deepest Leaves Sum

## Prompt
Given the root of a binary tree, return the sum of values of its deepest leaves.

## Example
![img](https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png)

Input: root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
Output: 15

## Explanation

![img](https://leetcode.com/problems/deepest-leaves-sum/Figures/1302/traversals.png)

![img](https://leetcode.com/problems/deepest-leaves-sum/Figures/1302/dfs_bfs2.png)

When asked to find information about a particular row of a binary tree, the normal thought is to use a breadth-first search (BFS) approach. A BFS approach usually involves the use of a queue data structure (q) so that we deal with the nodes of the tree in the proper order.

The trick is to deal with a single row at a time by making note of the length of the queue (qlen) when we start the row. Once we've processed that many nodes, we know we've just finished the current row and any remaining entries in q are from the next row. This can be accomplished easily with a nested loop.

In this case, processing a node simply means accumulating the running total (ans) for the row and then moving any children of the node onto the end of the queue.

When we start a new row, we can reset ans back to 0, and then just keep processing rows until q is empty. The last value of ans should be our final answer, so we should return ans.

Alternately, we can use a depth-first search (DFS) approach with recursion to traverse the binary tree. If we pass the row depth (lvl) as an argument to our recursive function (dfs), we can use it to update the values in an array of row sums (sums) by using lvl as an index (sums[lvl]). Then we can simply return the last value of sums as our answer.

## Solution 1 (bfs)

```js
var deepestLeavesSum = function(root) {
    let q = [root], ans, qlen, curr
    while (q.length) {
        qlen = q.length, ans = 0
        for (let i = 0; i < qlen; i++) {
            curr = q.shift(), ans += curr.val
            if (curr.left) q.push(curr.left)
            if (curr.right) q.push(curr.right)
        }
    }
    return ans
};
```

Time Complexity: O(N)

Space Complexity:O(N)

## Solution 2 (bfs)

```js
var deepestLeavesSum = function(root) {
   let queue = [root]
   let sum=0
   while (queue.length != 0){
      sum =0
      const n = queue.length
      
      for (let i = 0; i < n; i++){
         let curr = queue.pop()
         sum+=curr.val
         curr.left && queue.unshift(curr.left)
         curr.right && queue.unshift(curr.right)
      }
   }
   return sum
};
```

Time Complexity: O(N)

Space Complexity:O(N)

## Solution 3 (dfs)
```js
var deepestLeavesSum = function(root) {
    let sums = []
    const dfs = (node, lvl) => {
        if (lvl === sums.length) sums[lvl] = node.val
        else sums[lvl] += node.val
        if (node.left) dfs(node.left, lvl+1)
        if (node.right) dfs(node.right, lvl+1)
    }
    dfs(root, 0)
    return sums[sums.length-1]
};
```

Time Complexity: O(N)

Space Complexity: O(H) (height of the tree)

## Notes

For BFS: pop out from the left, first push the left child, and then the right one.

For DFS: pop out from the right, first push the right child, and then the left one.

Traverse the tree to find the max depth. (bfs or dfs)

Traverse the tree again to compute the sum required.
