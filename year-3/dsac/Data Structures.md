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

# Stack

# Queue

## Priority Queue

# Binary Search Tree

# Red Black Tree

# Graphs

