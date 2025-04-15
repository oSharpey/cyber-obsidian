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



# Hash Table

# Stack

# Queue

## Priority Queue

# Binary Search Tree

# Red Black Tree

# Graphs

