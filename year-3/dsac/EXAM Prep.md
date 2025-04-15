# Sorting Algorithms
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
