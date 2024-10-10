## Table of Contents
1. Introduction to Searching Algorithms
2. Linear Search
3. Binary Search
4. Variations of Binary Search
5. Comparison of Linear and Binary Search
6. Practical Applications
7. Practice Problems

## 1. Introduction to Searching Algorithms

Searching algorithms are designed to check for an element or retrieve an element from any data structure where it is stored. These algorithms are fundamental to computer science and are used in various applications, from simple data lookups to complex database operations.

### Key Concepts:
- **Search Space**: The collection of all items being searched.
- **Target**: The item being searched for.
- **Successful Search**: When the target is found.
- **Unsuccessful Search**: When the target is not in the search space.

## 2. Linear Search

Linear Search, also known as Sequential Search, is the simplest searching algorithm. It sequentially checks each element in the list until a match is found or the whole list has been searched.

### Algorithm:
1. Start from the leftmost element of the array.
2. Compare each element with the target value.
3. If the element matches the target, return its index.
4. If the end of the array is reached without finding a match, return -1 or a flag indicating the element wasn't found.

### Implementation in Python:
```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i  # Element found, return its index
    return -1  # Element not found
```

### Time Complexity:
- Worst-case: O(n) - when the target is at the end or not present
- Best-case: O(1) - when the target is at the beginning
- Average-case: O(n/2) ≈ O(n)

### Space Complexity:
- O(1) - only a constant amount of extra space is used

### Advantages:
1. Simple to understand and implement
2. Works on unsorted arrays
3. Efficient for small datasets

### Disadvantages:
1. Inefficient for large datasets
2. Linear time complexity makes it slow for big arrays

## 3. Binary Search

Binary Search is an efficient algorithm for searching a sorted array by repeatedly dividing the search interval in half.

### Prerequisites:
- The array must be sorted in ascending or descending order.

### Algorithm:
1. Compare the target value with the middle element of the array.
2. If they are equal, the search is successful.
3. If the target is less than the middle element, repeat the search on the left half.
4. If the target is greater than the middle element, repeat the search on the right half.
5. Repeat steps 1-4 until the element is found or it's clear the element is not present.

### Implementation in Python:
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid  # Element found, return its index
        elif arr[mid] < target:
            left = mid + 1  # Target is in the right half
        else:
            right = mid - 1  # Target is in the left half
    
    return -1  # Element not found
```

### Time Complexity:
- Worst-case: O(log n)
- Best-case: O(1) - when the middle element is the target
- Average-case: O(log n)

### Space Complexity:
- Iterative implementation: O(1)
- Recursive implementation: O(log n) due to the call stack

### Advantages:
1. Very efficient for large datasets
2. Logarithmic time complexity makes it much faster than linear search for big arrays

### Disadvantages:
1. Requires the array to be sorted
2. Not suitable for data structures that don't allow random access (e.g., linked lists)

## 4. Variations of Binary Search

### 4.1 Lower Bound (First Occurrence)

This variation finds the index of the first occurrence of a target value in a sorted array with duplicates.

```python
def lower_bound(arr, target):
    left, right = 0, len(arr) - 1
    result = -1
    
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            result = mid
            right = mid - 1  # Continue searching in the left half
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return result
```

### 4.2 Upper Bound (Last Occurrence)

This variation finds the index of the last occurrence of a target value in a sorted array with duplicates.

```python
def upper_bound(arr, target):
    left, right = 0, len(arr) - 1
    result = -1
    
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            result = mid
            left = mid + 1  # Continue searching in the right half
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return result
```

### 4.3 Rotated Sorted Array Search

This variation searches for a target value in a sorted array that has been rotated around a pivot point.

```python
def search_rotated(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        
        # Check which half is sorted
        if arr[left] <= arr[mid]:
            # Left half is sorted
            if arr[left] <= target < arr[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            # Right half is sorted
            if arr[mid] < target <= arr[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1
```

### 4.4 Search in Nearly Sorted Array

This variation searches for a target in an array where each element is at most k positions away from its position in the sorted array.

```python
def search_nearly_sorted(arr, target, k):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        
        # Check k elements on both sides
        for i in range(max(0, mid-k), min(len(arr), mid+k+1)):
            if arr[i] == target:
                return i
        
        if arr[mid] < target:
            left = mid + k + 1
        else:
            right = mid - k - 1
    
    return -1
```

## 5. Comparison of Linear and Binary Search

| Aspect                | Linear Search                   | Binary Search                    |
|-----------------------|---------------------------------|----------------------------------|
| Time Complexity       | O(n)                            | O(log n)                         |
| Space Complexity      | O(1)                            | O(1) iterative, O(log n) recursive |
| Sorted Data Required  | No                              | Yes                              |
| Flexibility           | Works with any data structure   | Requires random access           |
| Implementation        | Simple                          | More complex                     |
| Best for              | Small datasets, unsorted data   | Large datasets, sorted data      |

## 6. Practical Applications

### Linear Search:
1. Searching in small datasets
2. Searching in unsorted lists
3. Searching for a specific condition (not just equality)
4. Implementing other algorithms (e.g., string matching)

### Binary Search:
1. Searching in large, sorted datasets
2. Database indexing and searching
3. Finding roots of equations (e.g., Newton-Raphson method)
4. Optimizing algorithms in competitive programming

## 7. Practice Problems

1. Implement binary search recursively.
2. Find the number of occurrences of a target value in a sorted array with duplicates.
3. Search for an element in a sorted matrix where rows and columns are sorted.
4. Implement a function to find the square root of a number using binary search.
5. Given a sorted array of strings that is interspersed with empty strings, write a method to find the location of a given string.

Remember to analyze the time and space complexity of your solutions!

## Conclusion

Understanding these basic searching algorithms and their variations is crucial for solving more complex problems efficiently. As you progress in your algorithmic journey, you'll find these concepts forming the foundation for many advanced algorithms and data structures.