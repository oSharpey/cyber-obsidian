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



# Linked List

# Hash Table

# Stack

# Queue

## Priority Queue

# Binary Search Tree

# Red Black Tree

# Graphs

