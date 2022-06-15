# Duplicate Nodes in a Linked List
You are given the head of a singly linked list whose nodes are in sorted order with respect to their values. Write a function that will return a linked list that has had all the duplicates removed and the remaining nodes are still in sorted order with respect to their values.

Each `LinkedList` node has an integer `value` as well as a `next` node pointing to the next node in the list or to `null` if that is the tail of the list.

# Examples
Sample Input: 
`linkedList = 1 -> 1 -> 1 -> 3 -> 4 -> 4 ->4 -> 5 -> 6`

Sample Output:  
`1 -> 3 -> 4 -> 5 -> 6`

# Hints
1. The brute force approach uses a hash table or a set to keep track of all the node values that exist while traversing.
2. What does the fact that the nodes are sorted tell you about the location of all duplicate nodes?

# The General Idea
Step 1: First make an array(or set or object) that stores all the elements of the linked list. If you use an array make sure the array will have no duplicates. 
Step 2: Create a new linked list using the elements in the array(or set or object)

# Solution 1 - The Most Brute Force
```js
var deleteDuplicates = function(head) {
  const array = [];
  
  let current = head;
  //Loop through the linked list pushing each new value to the array we made earlier
  while (current) {
    if (!array.includes(current.val)) array.push(current.val);
    
    current = current.next;
  }
  
  //Make a new linked list making each node by looping through the array and making each stored value a new node.
  let headOfRemovedDuplicates = null;
  let pointerForRemovedDuplicates = null;
  for (const value of array) {
    if (!headOfRemovedDuplicates) {
      headOfRemovedDuplicates = new ListNode(value);
      pointerForRemovedDuplicates = headOfRemovedDuplicates;
    } else {
      pointerForRemovedDuplicates.next = new ListNode(value);
      pointerForRemovedDuplicates = pointerForRemovedDuplicates.next;
    }
  }
  
  return headOfRemovedDuplicates;
};
```
Time complexity: O(N^2) You have two loops. Each loop is N time where N is the length of the list.  
Space complexity: O(2N). You are creating two new places to store information; the array and the new linked list   

# Solution 1.5 - An Object This Time
```js
var deleteDuplicates = function(head) {
  // step 1: make an array (or set) that stores all the elements of the linked list. make sure it has no duplicates (well if you use a set or an object, it wont have duplicates anyways)
  const obj = {};
  
  let current = head;
  
  while (current) {
    // we dont really care about the values. we only care about the keys- which are the values of our linked list nodes
    obj[current.val] = true;
    
    current = current.next;
  }
  
  // step 2: create a new linked list using the elements in the array (or set or object)
  // this is the head of our linked list. we return this value
  let headOfRemovedDuplicates = null;

  // this is just a pointer to help us iterate the list. we do not want to iterate the head because that means we lost part of the list
  let tailOfRemovedDuplicates = null;

  // loop through the elements in the object. for each value, you want to "addToTail"
  Object.keys(obj).forEach(value => {
    // case 1 when creating our linked list: when the linked list is empty
    // if our linked list is empty, then we should define our head and our tail
    if (!headOfRemovedDuplicates) {
      headOfRemovedDuplicates = new ListNode(value);
      tailOfRemovedDuplicates = headOfRemovedDuplicates;

    // case 2 when creating our linked list: when the linked list is populated. we just want to add to the tail
    } else {
      tailOfRemovedDuplicates.next = new ListNode(value);
      tailOfRemovedDuplicates = tailOfRemovedDuplicates.next;
    }
  });
  
  return headOfRemovedDuplicates;
};
```
Time complexity: O(N) - Time is N because you're only using one `while` loop and no `includes` with your set
Space complexity: O(2N) - Same as the previous solution, you are creating a new array and a new linked list

# Solution 2 - Now Optimize
```js
var deleteDuplicates = function(head) {
    if (!head) return head;
    
    let current = head;
    let nextNonDupe = head.next;
    
    /*While current both of these are truthy we keep checking
    the next value to see if it is the same as our current value.
    Once the next value is different we've run out of duplicates and the process repeats*/
    while (current && nextNonDupe) {
        while (nextNonDupe && nextNonDupe.val === current.val) {
            nextNonDupe = nextNonDupe.next;
        }
        
        current.next = nextNonDupe;
        current = current.next;
    }
    
    return head;
};
```
Time complexity: O(N)
Space complexity: O(1) No extra space needed
