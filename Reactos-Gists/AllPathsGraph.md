# Prompt

Given a directed acyclic graph (DAG) of n nodes labeled from 0 to n - 1, find all possible paths from node 0 to node n - 1 and return them in any order.

The graph is given as follows: graph[i] is a list of all nodes you can visit from node i (i.e., there is a directed edge from node i to node graph[i][j]).

### Example 1
![Graph 1](https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg)  

```js
Input: graph = [ [ 1, 2 ], [ 3 ], [ 3 ], [] ]
                    0        1      2    3  // these are the index (vertex) numbers
Output: [ [ 0, 1, 3 ], [ 0, 2, 3 ] ]
```

```
(See how the indices of the matrix corresponds to the vertex number?)
The graph [ [ 1, 2 ], [ 3 ], [ 3 ], [] ] is basically like 
{
  0: [ 1, 2 ],
  1: [ 3 ],
  2: [ 3 ],
  3: [ ]
}
```

### Example 2
![Graph 2](https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg)

```js
Input: graph = [ [ 4, 3, 1 ], [ 3, 2, 4 ], [ 3 ], [ 4 ], [ ] ]
Output: [ [0, 4 ], [ 0, 3, 4 ], [ 0, 1, 3, 4 ], [0, 1, 2, 3, 4 ], [ 0, 1, 4 ] ]
```

# Code Example 1 (dfs)

```js
var allPathsSourceTarget = function(graph) {
    const target = graph.length - 1;
    
    const res = [];
    
    const DFS = (node,path) => {
        
        path.push(node);
        
	// if we've reached the target, we've found a path
        if(node === target) { 
            res.push(path);
            return;
        }
            
        for(let edge of graph[node]) {
            DFS(edge,[...path]);
        }
    }
    
    DFS(0,[]);
    
    return res;
};
```

Time complexity: O(V + E) V = # of vertices, E = # of edges.   
Space complexity: O(V)

# Code Example 2 (dfs)
```js
const getPaths = graph => {
  const allPaths = [];

  // graph, currentNode, allPaths, currentPath, destination
  // remember- startingVertex is always 0 so all paths 
  // will always start at 0 and end at destination!
  dfs(graph, 0, allPaths, [ 0 ], graph.length - 1);

  return allPaths;
};

const dfs = (graph, currentNode, allPaths, currentPath, destination) => {
  // if you're already at destination, push currentPath into allPaths
  if(currentNode === destination) {
    allPaths.push(currentPath);
  } else {
    // remember- currentNode is always the index, and the index # is always the vertex
    const neighbors = graph[currentNode];
    
    // just normal DFS
    for(const neighbor of neighbors) {
      dfs(graph, neighbor, allPaths, [...currentPath, neighbor], destination);
    }
  }
}

```
Time complexity: O(V + E) V = # of vertices, E = # of edges.   
Space complexity: O(V)


# Code Example 3 (bfs)
```js

const getPaths = graph => {
  // return value to contain all the paths
  const allPaths = [];

  // initialize a queue to hold all the paths. 
  // the path will ALWAYS start with 0- the starting node
  const queue = [[0]];
  
  // destination node is ALWAYS n - 1
  const destination = graph.length - 1;
  
  while(queue.length) {
    // get the current path from the queue
    const currentPath = queue.shift();
    const currentNode = currentPath[currentPath.length - 1];

    // if the last item in the current path is the destination,
    // push the current path into the return value paths
    if(currentNode === destination) {
      allPaths.push(currentPath);
    }

    // find all the neighbors of the current node that you're on
    for(const neighbor of graph[ currentNode ]) {
      // the queue holds all the paths that you already traversed
      // the new path will consist of: the entirety of the current path + the neighbor
      queue.push([...currentPath, neighbor]);
    }
  }

  return allPaths;
}

```

Time complexity: O(V + E) V = vertices, E = edges.  
Space complexity: O(V) 
