# Simple Sorting Algorithms
## Bubble Sort
### How it works
- **Step 1**: Start with the unsorted list and compare adjacent elements. Start from the first element, compare it with the next element. If the current element is greater than the next element, swap them.
- **Step 2**: Continue comparing the next pair of adjacent elements, swapping them if necessary. Repeat this process for every adjacent pair in the list.
- **Step 3**: At the end of one pass through the list, the largest element will have "bubbled up" to its correct position at the end of the list.
- **Step 4**: Ignore the last element (which is now sorted) and repeat the process for the remaining unsorted portion of the list. Continue this process until no swaps are needed, indicating the list is sorted.
### Big O
- Best Case Big-O: ***O(n)***
- Worst Case Big-O: ***O(n^2)***
- Average Big-O: ***O(n^2)***
- Space Complexity: ***O(1)***
#### Why these complexities
- Best case: 
	- If the array is already sorted
	- Algorithm will only have to loop through the array once
- Worst case:
	- Array is in reverse order
	- loops through every value in the array, comparing it to the value next to it. So for an array of n values, there must be n such comparisons in one loop.
	- And after one loop, the array is looped through again and again n times
	- Every single value will need to be bubbled up to the end of the array
### Optimised implementation
- If the algorithm loops through the whole array without swapping any values it must be sorted
```python
my_array = [7, 3, 9, 12, 11]

n = len(my_array)
for i in range(n-1):
    swapped = False
    for j in range(n-i-1):
        if my_array[j] > my_array[j+1]:
            my_array[j], my_array[j+1] = my_array[j+1], my_array[j]
            swapped = True
    if not swapped:
        break

print("Sorted array:", my_array)
```

## Insertion Sort
### How it works
- **Step 1**: Start with the first element and assume that the first element of the array is already sorted.
- **Step 2**: Select the next element in the array and compare it with the elements in the sorted portion (to its left).
- **Step 3**: If the new element is smaller than the elements in the sorted portion, shift the larger elements one position to the right to make space.
- **Step 4**: Insert the new element into the correct position in the sorted portion.
- **Step 5**: Move to the next unsorted element and repeat the process until all elements are sorted
### Big O
- Best Case Big-O: ***O(n)***
- Worst Case Big-O: ***O(n^2)***
- Average Big-O: ***O(n^2)***
- Space Complexity: ***O(1)***
#### Why these complexities
- Best case: 
	- If the array is already sorted
	- Algorithm will only have to loop through the array once
- Worst case:
	- Array is in reverse order
	- loops through every value in the array, comparing it every element in the sorted array before it 
	- Every single value will need to be inserted to the start of the array
- Average Case:
	- On average, each value must be compared to about n/2 other values to find the correct place to insert it.
	- Insertion Sort must run the loop to insert a value in its correct place approximately n times.
	- so we get O(n/2 * n) = O(n^2)
### Implementation
```python
my_array = [64, 34, 25, 12, 22, 11, 90, 5]

n = len(my_array)
for i in range(1,n):
    insert_index = i
    current_value = my_array.pop(i)
    for j in range(i-1, -1, -1):
        if my_array[j] > current_value:
            insert_index = j
    my_array.insert(insert_index, current_value)

print("Sorted array:", my_array)
```

## Selection Sort
### How it works
- **Step 1**: Start with the first element, assume the first element is the smallest in the list.
- **Step 2**: Traverse the unsorted part of the list and find the minimum value.
- **Step 3**: Swap the smallest element, swap this smallest element with the first element of the unsorted portion.
- **Step 4**: Move to the next element, repeat the process for the next element in the list, reducing the size of the unsorted portion by one after each iteration.
- **Step 5**: Repeat until sorted.
- ***Loops through the entire array, finds the smallest item and moves it to the front of the sorted section***
### Big O
- Best Case Big-O: ***O(n^2)***
- Worst Case Big-O: ***O(n^2)***
- Average Big-O: ***O(n^2)***
- Space Complexity: ***O(1)***
#### Why these complexities
- Regardless of the input order, the algorithm always performs a nested loop: one loop to pick the position in the array and a nested loop to find the minimum element
### Implementation
```python
my_array = [64, 34, 25, 5, 22, 11, 90, 12]

n = len(my_array)
for i in range(n-1):
    min_index = i
    for j in range(i+1, n):
        if my_array[j] < my_array[min_index]:
            min_index = j
    min_value = my_array.pop(min_index)
    my_array.insert(i, min_value)

print("Sorted array:", my_array)
```

# Advanced Sorting Algorithms 
## Merge Sort
### How it Works
**Step 1**: Arrays or lists are split into two (roughly )equal halves. Continue splitting recursively until each sublist contains a single element or is empty. These are inherently sorted.
**Step 2**: Merge each pair of sorted sublists into a single sorted sublist. Continue merging until all sublists are merged back into one sorted list. During the merge step, two sorted arrays are combined into one sorted array by comparing their elements one by one.
**Step 3**: After merging all sublists, the original list is fully sorted.

**Divide:** The algorithm starts with breaking up the array into smaller and smaller pieces until one such sub-array only consists of one element.
**Conquer:** The algorithm merges the small pieces of the array back together by putting the lowest values first, resulting in a sorted array.
![[Pasted image 20250415161325.png]]

### Big O
- Best Case Big-O: ***O(n * logn)***
- Worst Case Big-O: ***O(n * logn)***
- Average Big-O: ***O(n * logn)***
- Space Complexity: ***O(n)***
#### Why these complexities
- because the array is always divided in half and then merged.
- More space needed to hold individual sub lists
### Implementations
#### With Recursion
```python
def mergeSort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    leftHalf = arr[:mid]
    rightHalf = arr[mid:]

    sortedLeft = mergeSort(leftHalf)
    sortedRight = mergeSort(rightHalf)

    return merge(sortedLeft, sortedRight)

def merge(left, right):
    result = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])

    return result

unsortedArr = [3, 7, 6, -10, 15, 23.5, 55, -13]
sortedArr = mergeSort(unsortedArr)
print("Sorted array:", sortedArr)
```

#### Without Recursion
```python
def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
            
    result.extend(left[i:])
    result.extend(right[j:])
    
    return result

def mergeSort(arr):
    step = 1  # Starting with sub-arrays of length 1
    length = len(arr)
    
    while step < length:
        for i in range(0, length, 2 * step):
            left = arr[i:i + step]
            right = arr[i + step:i + 2 * step]
            
            merged = merge(left, right)
            
            # Place the merged array back into the original array
            for j, val in enumerate(merged):
                arr[i + j] = val
                
        step *= 2  # Double the sub-array length for the next iteration
        
    return arr

unsortedArr = [3, 7, 6, -10, 15, 23.5, 55, -13]
sortedArr = mergeSort(unsortedArr)
print("Sorted array:", sortedArr)
```

## Quick Sort
