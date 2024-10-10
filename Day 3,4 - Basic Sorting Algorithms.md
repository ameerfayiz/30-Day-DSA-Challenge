
## Table of Contents
1. Introduction to Sorting Algorithms
2. Bubble Sort
3. Selection Sort
4. Insertion Sort
5. Comparison of Basic Sorting Algorithms
6. Stability in Sorting Algorithms
7. Practical Applications
8. Practice Problems

## 1. Introduction to Sorting Algorithms

Sorting is the process of arranging a collection of items in a specific order, typically ascending or descending. Sorting algorithms are fundamental to computer science and form the basis for many other algorithms and data structures.

### Key Concepts:
- **In-place sorting**: Algorithms that don't require extra space proportional to the input size.
- **Stable sorting**: Algorithms that maintain the relative order of equal elements.
- **Comparison-based sorting**: Algorithms that sort by comparing elements.
- **Time complexity**: The number of operations an algorithm performs as a function of input size.
- **Space complexity**: The amount of extra memory an algorithm uses as a function of input size.

## 2. Bubble Sort

Bubble Sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order.

### Algorithm:
1. Start with the first element of the array.
2. Compare the current element with the next element.
3. If the current element is greater than the next element, swap them.
4. Move to the next element and repeat steps 2-3 until the end of the array.
5. Repeat steps 1-4 for each element in the array.

### Implementation in Python:
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        # Flag to optimize: if no swaps occur in a pass, the array is sorted
        swapped = False
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped:
            break
    return arr
```

### Time Complexity:
- Worst-case: O(n²) - when the array is reverse sorted
- Best-case: O(n) - when the array is already sorted (with optimization)
- Average-case: O(n²)

### Space Complexity:
- O(1) - in-place sorting algorithm

### Advantages:
1. Simple to understand and implement
2. In-place sorting (doesn't require extra space)
3. Stable sorting algorithm

### Disadvantages:
1. Very inefficient for large datasets
2. Quadratic time complexity makes it impractical for most real-world applications

## 3. Selection Sort

Selection Sort divides the input list into two parts: a sorted portion at the left end and an unsorted portion at the right end. It repeatedly selects the smallest (or largest) element from the unsorted portion and moves it to the sorted portion.

### Algorithm:
1. Find the minimum element in the unsorted portion of the array.
2. Swap it with the first element of the unsorted portion.
3. Move the boundary between the sorted and unsorted portions one element to the right.
4. Repeat steps 1-3 until the entire array is sorted.

### Implementation in Python:
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr
```

### Time Complexity:
- Worst-case: O(n²)
- Best-case: O(n²)
- Average-case: O(n²)

### Space Complexity:
- O(1) - in-place sorting algorithm

### Advantages:
1. Simple to understand and implement
2. In-place sorting (doesn't require extra space)
3. Makes the minimum number of swaps (O(n) swaps)

### Disadvantages:
1. Inefficient for large datasets
2. Always performs O(n²) comparisons, even if the array is already sorted
3. Not a stable sorting algorithm

## 4. Insertion Sort

Insertion Sort builds the final sorted array one item at a time. It iterates through an input array and removes one element per iteration, finds the location it belongs within the sorted list, and inserts it there.

### Algorithm:
1. Start with the second element of the array.
2. Compare it with the elements to its left.
3. If it's smaller than an element to its left, shift that element one position to the right.
4. Repeat step 3 until a smaller element is found or the start of the array is reached.
5. Insert the current element in the correct position.
6. Move to the next element and repeat steps 2-5 until the end of the array.

### Implementation in Python:
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr
```

### Time Complexity:
- Worst-case: O(n²) - when the array is reverse sorted
- Best-case: O(n) - when the array is already sorted
- Average-case: O(n²)

### Space Complexity:
- O(1) - in-place sorting algorithm

### Advantages:
1. Simple to understand and implement
2. In-place sorting (doesn't require extra space)
3. Stable sorting algorithm
4. Efficient for small datasets and nearly sorted arrays
5. Adaptive: O(n) time when array is nearly sorted

### Disadvantages:
1. Inefficient for large datasets
2. Quadratic time complexity for average and worst cases

## 5. Comparison of Basic Sorting Algorithms

| Aspect                | Bubble Sort    | Selection Sort | Insertion Sort |
|-----------------------|----------------|----------------|----------------|
| Time Complexity (Worst) | O(n²)          | O(n²)          | O(n²)          |
| Time Complexity (Best)  | O(n)           | O(n²)          | O(n)           |
| Time Complexity (Avg)   | O(n²)          | O(n²)          | O(n²)          |
| Space Complexity      | O(1)           | O(1)           | O(1)           |
| Stability             | Stable         | Not Stable     | Stable         |
| In-Place              | Yes            | Yes            | Yes            |
| Adaptivity            | Yes            | No             | Yes            |
| # of Swaps (Worst)    | O(n²)          | O(n)           | O(n²)          |

## 6. Stability in Sorting Algorithms

A sorting algorithm is said to be stable if two objects with equal keys appear in the same order in sorted output as they appear in the input array.

- Bubble Sort: Stable
- Selection Sort: Not Stable
- Insertion Sort: Stable

Example demonstrating stability:
```python
data = [(1, 'A'), (2, 'B'), (1, 'C'), (2, 'D')]
# Stable sort will maintain the order of 'A' before 'C' and 'B' before 'D'
# Unstable sort might change this order
```

## 7. Practical Applications

While these basic sorting algorithms are generally inefficient for large datasets, they have some practical uses:

1. **Educational purposes**: These algorithms are excellent for teaching sorting concepts and algorithm analysis.

2. **Small datasets**: For very small arrays (typically less than 10-20 elements), these algorithms can be faster than more complex algorithms due to lower overhead.

3. **Nearly sorted data**: Insertion sort performs well on nearly sorted data.

4. **Limited memory**: These algorithms are in-place, making them useful when memory is a constraint.

5. **Embedded systems**: In systems with limited computational power, simpler algorithms might be preferred.

6. **Hybrid algorithms**: Some advanced sorting algorithms use these basic sorts as subroutines for small partitions.

## 8. Practice Problems

1. Implement a function that uses bubble sort to sort an array of strings alphabetically.

2. Modify the selection sort algorithm to sort an array in descending order.

3. Implement a function that uses insertion sort to sort an array of objects based on a specific attribute.

4. Create a hybrid algorithm that uses insertion sort for small subarrays (less than 10 elements) and a more efficient algorithm for larger arrays.

5. Implement a function that counts the number of swaps performed by bubble sort and selection sort. Compare the results for various input arrays.

6. Create a visualization (using any method you prefer) that shows how each of these sorting algorithms progresses through an array.

Remember to analyze the time and space complexity of your solutions!

## Conclusion

Understanding these basic sorting algorithms is crucial for developing a strong foundation in algorithm design and analysis. While they may not be the most efficient for large-scale applications, the concepts they introduce (like in-place sorting, stability, and algorithm analysis) are fundamental to more advanced sorting algorithms and many other areas of computer science.

As you work through these algorithms, pay attention to how they manipulate the data, their efficiency in different scenarios, and how small changes in implementation can affect their performance. This understanding will be invaluable as you progress to more complex algorithms and data structures.