# Prompt
There are N rooms labeled from 0 to n - 1. All rooms are locked except for room 0 (your starting point is room 0). Your goal is to visit all of the rooms. However, you cannot enter a locked room without having its key.  
  
Each room has a set of keys that can unlock other rooms. 

*I will probably explain this problem to everyone before we start the reacto since it can be confusing*

# Input Format
You will be given a 2D array. Lets call this array `rooms`  
`rooms[i]` is the set of keys if you visited room `i`  

Example: 
```js
rooms   (this is the keys)           ->   [[1], [2], [3], []]
indices (this is the room number)    ->     0    1    2    3

If you go to room 0, you have access to the key for room 1
If you go to room 1, you have access to the key for room 2
If you go to room 2, you have access to the key for room 3
If you go to room 3, you have no keys
```

# Example 1
Input: `[[1], [2], [3], []]`    
  
![img](https://i.imgur.com/WPYMIbV.png)  
  
Output: true   
  
Explanation:  
We visit room 0, pick up key for room 1  
Visit room 1, pick up key for room 2  
Visit room 2, pick up key for room 3  
Visit room 3  
We were able to visit all the rooms, so we return true

# Example 2
Input: `[[1,3],[3,0,1],[2],[0]]`  
  
![img](https://i.imgur.com/Ym6iD9O.png)  

Output: false  

Explanation: We can never enter room 2 because the key for room 2 is inside of room 2. Have you ever left your car keys inside your car? :(  

# Hints
1. Most of graph problems is just understanding the format of the input. Make sure you understand the input before you attempt!
2. Stanley's favorite quote: 99% OF TREE PROBLEMS CAN BE SOLVED USING BFS/DFS! I know this isnt a tree but.... a tree is just a type of a graph
3. BFS!
4. DFS!
   
Im sure most people know bfs/dfs by now. The general algorithm for dfs goes as follows-
1. base case: when do you want to stop? in a tree's case, you'd stop if your node doesnt exist. in a graph, you'd stop if you've been in this room already
2. process the node
3. traverse the children

The general algorithm for bfs goes as follows-
1. make a queue with your starting point
2. while (queue.length)....
3. take out the first item of the queue
4. process the item
5. traverse the item's children
    
You can apply that here!  

# Solution 1: BFS
```js
const canVisitAllRooms = function(rooms) {
  // in a graph, you have to make sure to have a `visited` set
  // ill explain this on excalidraw but the reason is...
  // you dont need a visited set for a tree because a tree can only go downwards
  // meaning you can never go to the same treenode twice
  // however, graphs do not have the attribute that it will only go downwards. it can go
  // up, left, right, diagonal, back to itself etc etc
  // so you have to make sure not to visit the same node twice
  const visited = new Set();
  
  // step 1. initialize a queue with your starting point. we always start at the 0th room
  const queue = [0];
  
  // step 2. while queue.length...
  while (queue.length) {
    // step 3. take out the first item from the queue
    const node = queue.shift();
    
    // step 4. process the node (just add it to our visited set)
    visited.add(node);
    
    // step 5. traverse the neighbors
    for (const neighbor of rooms[node]) {
      if (!visited.has(neighbor)) queue.push(neighbor);
    }
  }
  
  // `visited` holds all the rooms you visited. check whether it's === to the # of rooms
  return visited.size === rooms.length;
};
```
Since this is a graph, you cant simply just say O(N) or O(N^2) because you have to take into account the # of rooms, AND # of keys  
Time complexity: O(R + K) R = # of rooms, K = # of keys. In general, you'd say O(V + E) V = vertices, E = edges  
Space complexity: O(R) R = # of rooms   

# Solution 2: DFS
```js
const canVisitAllRooms = function(rooms) {
    const visited = new Set();
    
    dfs(rooms, 0, visited);
    
    return visited.size === rooms.length;
};

const dfs = (allRooms, currentRoom, visited) => {
    // step 1. base case. stop if you've been in this room already
    if (visited.has(currentRoom)) return;

    // step 2. process the node
    visited.add(currentRoom);

    // step 3. traverse neighbors
    for (const neighbor of allRooms[currentRoom]) {
        dfs(allRooms, neighbor, visited);
    }
}
```
Time complexity: O(R + K) R = # of rooms, K = # keys   
Space: O(R) R = # rooms  
