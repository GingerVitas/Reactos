# Intersection of two linked lists
You are given two linked lists. They may or may not intersect. If they intersect, return the node at which they intersect. If they do not intersect, return `null`

# Examples
1. Notice how these two linked lists intersect at 8. Return the node with value `8` (return the node- not the value)
![img](https://imgur.com/jZ6tDnw.jpeg)
2. These two linked lists intersect at 2. Return the node with value `2`
![img](https://imgur.com/kDustEt.jpeg)
3. These two linked lists don't intersect at all. Return `null`
![img](https://i.imgur.com/3h2dzPu.jpg)

### If they really want to run their code in order to test whether their function works- you can give this code to them (typically in real interviews, you won't be able to run your code. you'd have to run through it line by line with your inputs)
```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

const head1 = new Node(4);
head1.next = new Node(1);

const intersection = new Node(8);
head1.next.next = intersection;

intersection.next = new Node(4);
intersection.next.next = new Node(5);

const head2 = new Node(5);
head2.next = new Node(6);
head2.next.next = new Node(1);
head2.next.next.next = intersection;

console.log(getIntersectionNode(head1, head2));
```

# Hints
1. You can do this by keeping track of the nodes that you've seen before. How can you keep track of nodes that you've seen before?
2. You have two choices-   
   a. Iterate through the first list. At every node, add the `visited` attribute. Iterate through the second linked list. When you see a node with the 'visited' flag, that is the intersection node.  
       
   b. You can use a `Set` to keep track of nodes that you've seen before https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set (see my linked list office hour to know more about sets in depth https://fullstackacademy.slack.com/archives/C02TEK20RHU/p1652307123035999). Iterate through the first linked list. Save every single node into the set. Iterate through the second linked list. If that node is in the set, that means that's the intersection point. 

# Solution 1
```js
const getIntersectionNode = (headA, headB) => {
    let head1 = headA;
    let head2 = headB;
    
    while(head1) {
        head1.visited = true;
        head1 = head1.next;
    }
    
    while(head2) {
        if (head2.visited) return head2;
        head2 = head2.next;
    }
    
    return null;
};
```
Time complexity: O(N + M) N = length of linked list 1. M = length of linked list 2  
Space complexity: O(N). You're saving a new attribute on every node of list 1   

# Solution 1.5
```js
const getIntersectionNode = (headA, headB) => {
    // initialize variables pointing to the heads of each list
    let head1 = headA;
    let head2 = headB;
    
    // in this set, we're storing nodes that we have already seen
    const visited = new Set();
    
    // add every single node from head1 into the set
    while (head1) {
        visited.add(head1);
        head1 = head1.next;
    }
    
    // iterate head2. if we've seen a node already, it means that is the intersection point
    while (head2) {
        if (visited.has(head2)) return head2;
        head2 = head2.next;
    }
    
    // if we havent returned by now, that means there 
    // is no intersection point so we return null
    return null;
};
```
Time complexity: O(N + M) N = length of linked list 1. M = length of linked list 2   
Space complexity: O(N) because you're saving every node of list 1 into the set    

# Follow up- how can we improve the space?
This is a bit more complex so don't worry if you dont understand it. I literally figured out this solution ~5 mins before writing this gist LOL  
  
I figured this out after realizing that if both linked lists are the same size, you can find the intersection point easily. If they're the same size, you can just iterate both linked lists at the same time. If you find that the two nodes are the same, that means you found the intersection node  
   
The algorithm goes as follows-
1. Count the number of nodes in the first linked list
2. Count the number of nodes in the second linked list
3. If the counts are equal, awesome! Skip step 4 and 5.
4. If countA is bigger than countB (this means linked list 1 is bigger than linked list 2), you'd have to "shrink" linked list 1 so that both linked lists are equal size
5. If countB is bigger than countA (linked list 2 bigger than linked list 1), "shrink" linked list 2 so that both linked lists are equal size
6. Both linked lists are same size now. Iterate both linked lists at the same time. If it hits `null` that means there is no intersection point. If the linked lists nodes are the same, that means you found the intersection point

# Solution 2
```js
const getIntersectionNode = (headA, headB) => {
    let countA = 0;
    let currentA = headA;
    
    while (currentA) {
        countA++;
        currentA = currentA.next;
    }
    
    let countB = 0;
    let currentB = headB;
    
    while (currentB) {
        countB++;
        currentB = currentB.next;
    }
    
    // countA is # of nodes in linked list 1
    // countB is # of nodes in linked list 2
    
    // reset currentA and currentB to the heads of the lists
    currentA = headA;
    currentB = headB;
    
    // if list 2 is smaller than list 1,
    // you'd have to "shrink" list 1 by "removing the head" until the sizes are the same
    while (countB < countA) {
        currentA = currentA.next;
        countA--;
    }
    
    // if list 1 is smaller than list 2,
    // "shrink" list 2 by "removing the head" until the sizes are the same
    while (countA < countB) {
        currentB = currentB.next;
        countB--;
    }
    
    // now, the sizes of both lists are the same. we can simply iterate one node at a time
    // until we find both nodes that are equal
    while (currentA && currentB) {
        if (currentA === currentB) return currentA;
        currentA = currentA.next;
        currentB = currentB.next;
    }
    
    // if you reach here, that means you reached the end of the lists
    // without finding an intersection point
    return null;
};
```
Time complexity: O(N + M) (even though there are a ton of while loops, you're only iterating the entire linked list like twice)  
Space complexity: O(1) no extra space needed :) 
