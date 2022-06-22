# Biggest Area of Island

## Prompt
Given a 2d array board of only 0's and 1's, an island is a group of 1's all connected together. **Diagonals do not count as connected.** Find the maximum area of an island given the board.

### Example 1
Test Input:   
![Island Image 1](https://i.imgur.com/97CYsB3.png)  

Input: 
```js
const board = [
    [0,0,1,0,0,0,0,1,0,0,0,0,0],
    [0,0,0,0,0,0,0,1,1,1,0,0,0],
    [0,1,1,0,1,0,0,0,0,0,0,0,0],
    [0,1,0,0,1,1,0,0,1,0,1,0,0],
    [0,1,0,0,1,1,0,0,1,1,1,0,0],
    [0,0,0,0,0,0,0,0,0,0,1,0,0],
    [0,0,0,0,0,0,0,1,1,1,0,0,0],
    [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
```
Output: ```6```   

### Example 2
Test Input:   
![Island Image 2](https://i.imgur.com/j2QGtAe.png)  
```js
const board = [
    [1,1,0],
    [1,0,1],
    [0,1,0]
]
```

Output: ```3```

### Example 3
Test Input:   
![Island Image 3](https://i.imgur.com/pVAYu6S.png)  
^
it's not the 3 diagonals because diagonals don't count as connected  

```js
const board = [
    [1,0,0,0,1],
    [0,1,0,0,1],
    [0,0,1,0,0]
]
```
Output: ```2```

## Hints
1. You'd need to traverse through the whole board. How would you traverse a board like this?
2. Remember BFS? You should! BFS isn't just used to traverse trees. You can use it to traverse anything.
3. You can also use DFS if you want to.

## Solutions

### BFS Solution
```js
const maxAreaOfIsland = grid => {
    // global max
    let maxArea = 0;
    
    for(let i = 0; i < grid.length; i++) {
        for(let j = 0; j < grid[i].length; j++) {
            // only start searching if the plot 
            // you're currently on is land
            if(grid[i][j] === 1) {
                // local max
                const currentMax = bfs(grid, i, j);
                maxArea = Math.max(maxArea, currentMax)
            }
        }
    }
    
    // return the global max
    return maxArea;
};

const bfs = (grid, row, col) => {
    const queue = [[ row, col ]];
    
    // area starts off at 0 because you did not count the initial
    // plot yet. you will count it after you shift from queue
    // for the first time
    let area = 0;
    
    while(queue.length) {
        // take off the first coords from the queue
        const [ currentRow, currentCol ] = queue.shift();
        
        // if you hit ocean, stop this path from going any further
        if(grid[currentRow][currentCol] === 0) continue;
        
        // mark as 'visited'. if you dont mark as visited, 
        // the current path will never stop because 
        // it'll just go back to the spot it just was
        // aka infinite loop
        grid[currentRow][currentCol] = 0;
        
        // if it hit this line, that means the current 
        // spot is definitely land, so you add 1 to the area
        area++;
        
        if(currentRow > 0) queue.push([ currentRow - 1, currentCol ]);  // go up 
        if(currentCol > 0) queue.push([ currentRow, currentCol - 1 ]); // go left
        if(currentRow < grid.length - 1) queue.push([ currentRow + 1, currentCol ]); // go down
        if(currentCol < grid[currentRow].length - 1) queue.push([ currentRow, currentCol + 1 ]); // go right
        /*
            [(i - 1, j - 1), (i - 1, j), (i - 1,  j + 1)]
            [(i   ,  j - 1), (i   ,  j), (  i,  j + 1  )]
            [(i + 1, j - 1), (i + 1, j), (i + 1,  j + 1)]
        */
    }
    return area;
}
```
Time complexity: O(N * M) N = number of rows, M = number of columns   
Space complexity: O(N * M) N = number of rows, M = number of columns     

### DFS Solution
```js

const maxAreaOfIsland = grid => {
    // global max
    let maxArea = 0;
    
    for(let i = 0; i < grid.length; i++) {
        for(let j = 0; j < grid[i].length; j++) {
            // only start searching if the plot 
            // you're currently on is land
            if(grid[i][j] === 1) {
                // local max
                const currentMax = dfs(grid, i, j);
                maxArea = Math.max(maxArea, currentMax)
            }
        }
    }
    
    // return the global max
    return maxArea;
};

const dfs = (grid, row, col) => {
    const outOfBounds = row < 0 || col < 0 || row >= grid.length || col >= grid[row].length;
    if(outOfBounds || grid[row][col] === 0) return 0; // if out of bounds or if you hit ocean, return 0
    grid[row][col] = 0; // mark as visited. same logic as BFS solution
    
    let count = 1; // start count at 1 here because you didn't count the initial spot yet
    
    count += dfs(grid, row - 1, col); // same logic as BFS solution
    count += dfs(grid, row + 1, col);
    count += dfs(grid, row, col + 1);
    count += dfs(grid, row, col - 1);
    
    return count;
}

```

Time complexity: O(N * M) N = rows, M = columns  
Space complexity: O(N * M) because worst case, you'll have every single item in the recursion stack

### Extra Notes
In the above solution, you can see some sort of repetition in this block of code 
```js
count += dfs(grid, row - 1, col); // same logic as BFS solution
count += dfs(grid, row + 1, col);
count += dfs(grid, row, col + 1);
count += dfs(grid, row, col - 1);
```
There's a simple way to avoid this repetition- 
```js

//                     up     down    left     right
const directions = [[-1, 0], [1, 0], [0, -1], [0, 1]];
for (const [ x, y ] of directions) {
   count += dfs(grid, row + x, col + y);
}
```
This goes in all 4 directions. a lot of interviewers prefer this way because if you want to change it so that you can go diagonally, you'd just add the directions into this array like this- 
`const directions = [[-1, 0], [1, 0], [0, -1], [0, 1], [-1, -1], [-1, 1], [1, -1], [1, 1]];`
