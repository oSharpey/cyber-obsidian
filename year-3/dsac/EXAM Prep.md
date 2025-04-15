# Sorting Algorithms
## Bubble Sort
### Big O
- Best Case Big-O: ***O(n)***
- Worst Case Big-O: ***O(n^2)***
- Average Big-O: ***O(n^2)***
- Space Complexity: ***O(1)***
#### Why 
### Un-optimised implementation
```python
my_array = [64, 34, 25, 12, 22, 11, 90, 5]

n = len(my_array)
for i in range(n-1):
    for j in range(n-i-1):
        if my_array[j] > my_array[j+1]:
            my_array[j], my_array[j+1] = my_array[j+1], my_array[j]

print("Sorted array:", my_array)
```
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