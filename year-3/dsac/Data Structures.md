# Array
- **Insertion:**
    - _End Insertion:_  
        When appending an element to the end (assuming the underlying storage has extra capacity), this is an **O(1)** operation. However, if the array must be resized, the amortized cost is still constant time.
    - _Insertion at Arbitrary Position:_  
        Inserting at any position other than the end requires shifting all subsequent elements one index forward, costing **O(n)** in the worst case.
- **Searching by Value:**
    - For an unsorted array, a linear search is needed, making it **O(n)** in the worst case.
    - For a sorted array, binary search can be employed, which costs **O(log n)**.
- **Searching by Index:**
    - Arrays allow **O(1)** access by index due to contiguous memory allocation.


```python
# Find lowest valye in array
my_array = [7, 12, 9, 4, 11]
minVal = my_array[0]    # Step 1

for i in my_array:      # Step 2
    if i < minVal:      # Step 3
        minVal = i
        
print('Lowest value: ',minVal) # Step 4
```

# Linked List
A linked list consists of nodes with some sort of data, and a pointer, or link, to the next node.

![A singly linked list.](https://www.w3schools.com/dsa/img_linkedlists_singly.svg)

A big benefit with using linked lists is that nodes are stored wherever there is free space in memory, the nodes do not have to be stored contiguously right after each other like elements are stored in arrays. Another nice thing with linked lists is that when adding or removing nodes, the rest of the nodes in the list do not have to be shifted.

|                                                                                                             | Arrays | Linked Lists |
| ----------------------------------------------------------------------------------------------------------- | ------ | ------------ |
| _An existing data structure in the programming language_                                                    | Yes    | No           |
| _Fixed size in memory_                                                                                      | Yes    | No           |
| _Elements, or nodes, are stored right after each other in memory (contiguously)_                            | Yes    | No           |
| _Memory usage is low  <br>(each node only contains data, no links to other nodes)_                          | Yes    | No           |
| _Elements, or nodes, can be accessed directly (random access)_                                              | Yes    | No           |
| _Elements, or nodes, can be inserted or deleted in constant time, no shifting operations in memory needed._ | No     | Yes          |
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
    
node1 = Node(3)
node2 = Node(5)
node3 = Node(13)
node4 = Node(2)

node1.next = node2
node2.next = node3
node3.next = node4

currentNode = node1
while currentNode:
    print(currentNode.data, end=" -> ")
    currentNode = currentNode.next
print("null")
```
## Traversal of a Linked List
- Traversing a linked list means to go through the linked list by following the links from one node to the next.
- Traversal of linked lists is typically done to search for a specific node, and read or modify the node's content, remove the node, or insert a node right before or after that node.
- To traverse a singly linked list, we start with the first node in the list, the head node, and follow that node's next link, and the next node's next link and so on, until the next address is null
- **Searching by Value:**
    - A linear traversal is required, so the complexity is **O(n)**.
- **Searching by Index:**
    - To access a node at a particular index, you need to traverse the list from the head, which is **O(n)** in the worst-case.
```python
def traverseAndPrint(head):
    currentNode = head
    while currentNode:
        print(currentNode.data, end=" -> ")
        currentNode = currentNode.next
    print("null")
```
## Delete a Node in a Linked List
- In this case we have the link (or pointer or address) to a node that we want to delete.
- It is important to connect the nodes on each side of the node before deleting it, so that the linked list is not broken.
- So before deleting the node, we need to get the next pointer from the previous node, and connect the previous node to the new next node before deleting the node in between.
- In a singly linked list, like we have here, to get the next pointer from the previous node we actually need traverse the list from the start, because there is no way to go backwards from the node we want to delete.
```python
def deleteSpecificNode(head, nodeToDelete):
    if head == nodeToDelete:
        return head.next
    currentNode = head
    while currentNode.next and currentNode.next != nodeToDelete:
        currentNode = currentNode.next
    if currentNode.next is None:
        return head
    currentNode.next = currentNode.next.next
    return head
```
## Insert a Node in a Linked List
Inserting a node into a linked list is very similar to deleting a node, because in both cases we need to take care of the next pointers to make sure we do not break the linked list.
To insert a node in a linked list we first need to create the node, and then at the position where we insert it, we need to adjust the pointers so that the previous node points to the new node, and the new node points to the correct next node.
**Insertion:**
- **Best Case:**  
    Inserting at the beginning or immediately after a known node is **O(1)** (for both singly and doubly linked lists).
- **Worst Case:**  
    Inserting at the end (if you do not maintain a tail pointer) is **O(n)** because you must traverse the entire list.
    
```python
def insertNodeAtPosition(head, newNode, position):
    if position == 1:
        newNode.next = head
        return newNode
    
    currentNode = head
    for _ in range(position - 2):
        if currentNode is None:
            break
        currentNode = currentNode.next

    newNode.next = currentNode.next
    currentNode.next = newNode
    return head
```
# Hash Table
- Hash Table elements are stored in storage containers called **buckets**.
- Every Hash Table element has a part that is unique that is called the **key**.
- A **hash function** takes the key of an element to generate a **hash code**.
- The hash code says what bucket the element belongs to, so now we can go directly to that Hash Table element: to modify it, or to delete it, or just to check if it exists. Specific hash functions are explained in detail on the next two pages.
- A **collision** happens when two Hash Table elements have the same hash code, because that means they belong to the same **bucket**. A collision can be solved in two ways.
- **Chaining** is the way collisions are solved in this tutorial, by using arrays or linked lists to allow more than one element in the same bucket.
- **Open Addressing** is another way to solve collisions. With open addressing, if we want to store an element but there is already an element in that bucket, the element is stored in the next available bucket. This can be done in many different ways, but we will not explain open addressing any further here.

**Overview and Operation Complexities:**
- **Common-Case (Average-Case):**
    - **Insertion:** **O(1)**
    - **Search:** **O(1)**
    - **Deletion:** **O(1)**
- **Collision Resolution Techniques:**
    - **Chaining:**  
        Each bucket holds a secondary data structure (like a linked list) where multiple items that hash to the same index are stored.
        - _Worst-Case:_ If many keys collide (e.g., a poor hash function), all keys could be stored in one bucket, leading to **O(n)** operations.
    - **Open Addressing:**  
        When a collision occurs, the algorithm searches for the next available slot following a probe sequence (linear, quadratic, or double hashing).
        - _Worst-Case:_ Again, if many keys cluster together, operations may degrade to **O(n)**.
```python
my_hash_set = [
    [None],
    ['Jones'],
    [None],
    ['Lisa'],
    [None],
    ['Bob'],
    [None],
    ['Siri'],
    ['Pete'],
    [None]
]

def hash_function(value):
    return sum(ord(char) for char in value) % 10
    
def add(value):
    index = hash_function(value)
    bucket = my_hash_set[index]
    if value not in bucket:
        bucket.append(value)
        
def contains(value):
    index = hash_function(value)
    bucket = my_hash_set[index]
    return value in bucket

add('Stuart')

print(my_hash_set)
print('Contains Stuart:',contains('Stuart'))
```
# Stack
- Think of a stack like a pile of pancakes.
- In a pile of pancakes, the pancakes are both added and removed from the top. So when removing a pancake, it will always be the last pancake you added. This way of organizing elements is called LIFO: Last In First Out.
- Basic operations we can do on a stack are:
	- **Push:** Adds a new element on the stack.
	- **Pop:** Removes and returns the top element from the stack.
	- **Peek:** Returns the top element on the stack.
	- **isEmpty:** Checks if the stack is empty.
	- **Size:** Finds the number of elements in the stack.
- Stacks can be implemented by using arrays or linked lists.1
- Stacks can be used to implement undo mechanisms, to revert to previous states, to create algorithms for depth-first search in graphs, or for backtracking.
#### With array
```python
class Stack:
    def __init__(self):
        self.stack = []
    
    def push(self, element):
        self.stack.append(element)
    
    def pop(self):
        if self.isEmpty():
            return "Stack is empty"
        return self.stack.pop()
    
    def peek(self):
        if self.isEmpty():
            return "Stack is empty"
        return self.stack[-1]
    
    def isEmpty(self):
        return len(self.stack) == 0
    
    def size(self):
        return len(self.stack)

# Create a stack
myStack = Stack()

myStack.push('A')
myStack.push('B')
myStack.push('C')
print("Stack: ", myStack.stack)

print("Pop: ", myStack.pop())

print("Peek: ", myStack.peek())

print("isEmpty: ", myStack.isEmpty())

print("Size: ", myStack.size())
```
#### With Linked list
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

class Stack:
    def __init__(self):
        self.head = None
        self.size = 0
    
    def push(self, value):
        new_node = Node(value)
        if self.head:
            new_node.next = self.head
        self.head = new_node
        self.size += 1
    
    def pop(self):
        if self.isEmpty():
            return "Stack is empty"
        popped_node = self.head
        self.head = self.head.next
        self.size -= 1
        return popped_node.value
    
    def peek(self):
        if self.isEmpty():
            return "Stack is empty"
        return self.head.value
    
    def isEmpty(self):
        return self.size == 0
    
    def stackSize(self):
        return self.size

myStack = Stack()
myStack.push('A')
myStack.push('B')
myStack.push('C')

print("Pop: ", myStack.pop())
print("Peek: ", myStack.peek())
print("isEmpty: ", myStack.isEmpty())
print("Size: ", myStack.stackSize())
```

**Overview and Operation Complexities:**
- **Stack (LIFO – Last In, First Out):**
    - **Insertion (Push):** **O(1)**
    - **Search by Value:**  
        Typically **O(n)** in the worst and average cases, since you may have to check each element.
    - **Deletion (Pop):** **O(1)**
# Queue
Think of a queue as people standing in line in a supermarket.
The first person to stand in line is also the first who can pay and leave the supermarket. This way of organizing elements is called FIFO: First In First Out.
Basic operations we can do on a queue are:
- **Enqueue:** Adds a new element to the queue.
- **Dequeue:** Removes and returns the first (front) element from the queue.
- **Peek:** Returns the first element in the queue.
- **isEmpty:** Checks if the queue is empty.
- **Size:** Finds the number of elements in the queue.
#### With array
```python
class Queue:
    def __init__(self):
        self.queue = []
    
    def enqueue(self, element):
        self.queue.append(element)
    
    def dequeue(self):
        if self.isEmpty():
            return "Queue is empty"
        return self.queue.pop(0)
    
    def peek(self):
        if self.isEmpty():
            return "Queue is empty"
        return self.queue[0]
    
    def isEmpty(self):
        return len(self.queue) == 0
    
    def size(self):
        return len(self.queue)

# Create a queue
myQueue = Queue()

myQueue.enqueue('A')
myQueue.enqueue('B')
myQueue.enqueue('C')
print("Queue: ", myQueue.queue)

print("Dequeue: ", myQueue.dequeue())

print("Peek: ", myQueue.peek())

print("isEmpty: ", myQueue.isEmpty())

print("Size: ", myQueue.size())
```
#### With linked list
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class Queue:
    def __init__(self):
        self.front = None
        self.rear = None
        self.length = 0
    
    def enqueue(self, element):
        new_node = Node(element)
        if self.rear is None:
            self.front = self.rear = new_node
            self.length += 1
            return
        self.rear.next = new_node
        self.rear = new_node
        self.length += 1
    
    def dequeue(self):
        if self.isEmpty():
            return "Queue is empty"
        temp = self.front
        self.front = temp.next
        self.length -= 1
        if self.front is None:
            self.rear = None
        return temp.data
    
    def peek(self):
        if self.isEmpty():
            return "Queue is empty"
        return self.front.data
    
    def isEmpty(self):
        return self.length == 0
    
    def size(self):
        return self.length

    def printQueue(self):
        temp = self.front
        while temp:
            print(temp.data, end=" ")
            temp = temp.next
        print()

# Create a queue
myQueue = Queue()

myQueue.enqueue('A')
myQueue.enqueue('B')
myQueue.enqueue('C')
print("Queue: ", end="")
myQueue.printQueue()

print("Dequeue: ", myQueue.dequeue())

print("Peek: ", myQueue.peek())

print("isEmpty: ", myQueue.isEmpty())

print("Size: ", myQueue.size())
```
### Complexities
**Queue (FIFO – First In, First Out):**
- **Insertion (Enqueue):** **O(1)**
- **Search by Value:** **O(n)**
- **Deletion (Dequeue):** **O(1)**
## Priority Queue
- A priority queue is an abstract data structure in which each element has a priority.
- It allows for the retrieval of the highest (or lowest) priority element rather than just the "first in" or "last in" element.
**Basic Implementations:**
- **Using an Array/Linked List:**  
    One simple implementation is to keep items sorted upon insertion. This makes insertion cost **O(n)**, but removal of the highest-priority element is **O(1)**.
- **Using a Heap (not examining binary heaps per se):**  
    A more efficient (and common) implementation is to use a heap (often a binary heap). This is typically how priority queues are implemented in practice but you’re not required to know exact complexities here.
- **Understanding:**  
    The key takeaway is that priority queues allow you to quickly retrieve an element based on its priority rather than its insertion order.

# Binary Tree
A Binary Tree is a type of tree data structure where each node can have a maximum of two child nodes, a left child node and a right child node.
This restriction, that a node can have a maximum of two child nodes, gives us many benefits:
- Algorithms like traversing, searching, insertion and deletion become easier to understand, to implement, and run faster.
- Keeping data sorted in a Binary Search Tree (BST) makes searching very efficient.
- Balancing trees is easier to do with a limited number of child nodes, using an AVL Binary Tree for example.
- Binary Trees can be represented as arrays, making the tree more memory efficient.

A **parent** node, or **internal** node, in a Binary Tree is a node with one or two **child** nodes.
The **left child node** is the child node to the left.
The **right child node** is the child node to the right.
The **tree height** is the maximum number of edges from the root node to a leaf node.

### Binary Trees vs Arrays and Linked Lists
Benefits of Binary Trees over Arrays and Linked Lists:
- **Arrays** are fast when you want to access an element directly, like element number 700 in an array of 1000 elements for example. But inserting and deleting elements require other elements to shift in memory to make place for the new element, or to take the deleted elements place, and that is time consuming.
- **Linked Lists** are fast when inserting or deleting nodes, no memory shifting needed, but to access an element inside the list, the list must be traversed, and that takes time.
- **Binary Trees**, such as Binary Search Trees and AVL Trees, are great compared to Arrays and Linked Lists because they are BOTH fast at accessing a node, AND fast when it comes to deleting or inserting a node, with no shifts in memory needed.

We will take a closer look at how Binary Search Trees (BSTs) and AVL Trees work on the next two pages, but first let's look at how a Binary Tree can be implemented, and how it can be traversed.

### Types of Binary Trees
There are different variants, or types, of Binary Trees worth discussing to get a better understanding of how Binary Trees can be structured.
The different kinds of Binary Trees are also worth mentioning now as these words and concepts will be used later in the tutorial.
Below are short explanations of different types of Binary Tree structures, and below the explanations are drawings of these kinds of structures to make it as easy to understand as possible.
- A **balanced** Binary Tree has at most 1 in difference between its left and right subtree heights, for each node in the tree.
- A **complete** Binary Tree has all levels full of nodes, except the last level, which is can also be full, or filled from left to right. The properties of a complete Binary Tree means it is also balanced.
- A **full** Binary Tree is a kind of tree where each node has either 0 or 2 child nodes.
- A **perfect** Binary Tree has all leaf nodes on the same level, which means that all levels are full of nodes, and all internal nodes have two child nodes.The properties of a perfect Binary Tree means it is also full, balanced, and complete.
## Binary Search Tree

# Red Black Tree

# Graphs

