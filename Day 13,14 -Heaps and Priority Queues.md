## Table of Contents
1. Introduction to Heaps
2. Types of Heaps
   2.1 Min-Heap
   2.2 Max-Heap
3. Heap Properties and Structure
4. Heap Operations
   4.1 Insertion
   4.2 Deletion
   4.3 Heapify
   4.4 Build Heap
   4.5 Heap Sort
5. Priority Queues
6. Applications of Heaps and Priority Queues
7. Comparison with Other Data Structures
8. Advanced Heap Variations
9. Practice Problems

## 1. Introduction to Heaps

A heap is a specialized tree-based data structure that satisfies the heap property. Heaps are commonly used to implement priority queues, which are essential in various algorithms like Dijkstra's shortest path, Huffman coding, and more.

Key Characteristics:
- Complete Binary Tree: All levels are fully filled except possibly the last level, which is filled from left to right.
- Heap Property: The key of each node is either greater than or equal to (max-heap) or less than or equal to (min-heap) the keys of its children.

## 2. Types of Heaps

### 2.1 Min-Heap

In a min-heap, the root node has the minimum value in the heap, and this property is recursively true for all sub-heaps.

Example of a min-heap:
```
       1
     /   \
    3     2
   / \   /
  6   5 4
```

### 2.2 Max-Heap

In a max-heap, the root node has the maximum value in the heap, and this property is recursively true for all sub-heaps.

Example of a max-heap:
```
       9
     /   \
    7     8
   / \   /
  6   5 3
```

## 3. Heap Properties and Structure

1. Shape Property: A heap is a complete binary tree.
2. Heap Property: 
   - For max-heap: parent.key ≥ child.key
   - For min-heap: parent.key ≤ child.key
3. Array Representation: Heaps are typically implemented using arrays, where for a node at index i:
   - Left child: 2i + 1
   - Right child: 2i + 2
   - Parent: (i - 1) // 2

## 4. Heap Operations

### 4.1 Insertion

To insert a new element into a heap:
1. Add the element to the bottom level of the heap at the leftmost open space.
2. Compare the added element with its parent; if they are in the correct order, stop.
3. If not, swap the element with its parent and return to the previous step.

Time Complexity: O(log n)

Python implementation for max-heap insertion:

```python
def insert(heap, key):
    heap.append(key)
    current = len(heap) - 1
    while current > 0 and heap[(current - 1) // 2] < heap[current]:
        heap[current], heap[(current - 1) // 2] = heap[(current - 1) // 2], heap[current]
        current = (current - 1) // 2
```

### 4.2 Deletion

To delete the root from a heap:
1. Replace the root of the heap with the last element on the last level.
2. Compare the new root with its children; if they are in the correct order, stop.
3. If not, swap the element with one of its children and return to the previous step.
   (Swap with the smaller child in a min-heap and the larger child in a max-heap.)

Time Complexity: O(log n)

Python implementation for max-heap deletion:

```python
def delete_max(heap):
    if not heap:
        return None
    if len(heap) == 1:
        return heap.pop()
    
    max_val = heap[0]
    heap[0] = heap.pop()
    current = 0
    while True:
        largest = current
        left = 2 * current + 1
        right = 2 * current + 2
        
        if left < len(heap) and heap[left] > heap[largest]:
            largest = left
        if right < len(heap) and heap[right] > heap[largest]:
            largest = right
        
        if largest == current:
            break
        
        heap[current], heap[largest] = heap[largest], heap[current]
        current = largest
    
    return max_val
```

### 4.3 Heapify

Heapify is the process of creating a heap from an array or adjusting the heap after a change. There are two main types:

1. Heapify-up (also called bubble-up or sift-up): Used when inserting a new element.
2. Heapify-down (also called bubble-down or sift-down): Used when deleting the root element.

Python implementation of heapify-down for max-heap:

```python
def heapify_down(heap, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2

    if left < n and heap[left] > heap[largest]:
        largest = left
    if right < n and heap[right] > heap[largest]:
        largest = right

    if largest != i:
        heap[i], heap[largest] = heap[largest], heap[i]
        heapify_down(heap, n, largest)
```

Time Complexity: O(log n)

### 4.4 Build Heap

To build a heap from an unordered array:
1. Start from the last non-leaf node (n/2 - 1, where n is the number of elements).
2. Perform heapify-down on each node, moving towards the root.

Python implementation for building a max-heap:

```python
def build_max_heap(arr):
    n = len(arr)
    for i in range(n // 2 - 1, -1, -1):
        heapify_down(arr, n, i)
```

Time Complexity: O(n)

### 4.5 Heap Sort

Heap Sort uses a heap to sort an array in ascending (using max-heap) or descending (using min-heap) order.

Algorithm:
1. Build a max-heap from the input array.
2. Swap the root (maximum element) with the last element of the heap.
3. Reduce the heap size by 1 and heapify the root.
4. Repeat steps 2-3 until the heap size is 1.

Python implementation of Heap Sort:

```python
def heap_sort(arr):
    n = len(arr)
    
    # Build max-heap
    for i in range(n // 2 - 1, -1, -1):
        heapify_down(arr, n, i)
    
    # Extract elements from the heap one by one
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify_down(arr, i, 0)
```

Time Complexity: O(n log n)

## 5. Priority Queues

A priority queue is an abstract data type similar to a regular queue or stack, where each element has a "priority" associated with it. In a priority queue, an element with high priority is served before an element with low priority.

Heaps are an efficient implementation of priority queues:
- Min-heap for min-priority queue
- Max-heap for max-priority queue

Operations:
- Insert: Add an element (O(log n))
- Delete-max/Delete-min: Remove and return the highest/lowest priority element (O(log n))
- Peek: View the highest/lowest priority element without removing it (O(1))

## 6. Applications of Heaps and Priority Queues

1. Dijkstra's algorithm for finding the shortest path
2. Prim's algorithm for Minimum Spanning Tree
3. Huffman coding for data compression
4. Selection algorithms (e.g., finding the k-th smallest element)
5. Event-driven simulation (e.g., customers in a line)
6. Task scheduling in operating systems
7. Median maintenance

## 7. Comparison with Other Data Structures

| Operation | Array (unsorted) | Array (sorted) | Binary Search Tree | Heap |
|-----------|------------------|----------------|---------------------|------|
| Insert    | O(1)             | O(n)           | O(log n) average    | O(log n) |
| Delete    | O(n)             | O(n)           | O(log n) average    | O(log n) |
| Find Min/Max | O(n)          | O(1)           | O(log n)            | O(1) |
| Search    | O(n)             | O(log n)       | O(log n) average    | O(n) |

## 8. Advanced Heap Variations

1. Binomial Heap: A heap-like data structure that supports faster union operations
2. Fibonacci Heap: Provides better amortized performance for some operations
3. Leftist Heap: A variant of binary heap that supports faster merging
4. Skew Heap: A self-adjusting form of leftist heap
5. Pairing Heap: A self-adjusting heap with good amortized performance

## 9. Practice Problems

1. Implement a min-heap class with insert, delete_min, and peek operations.
2. Given an array of n elements, find the k-th largest element using a heap.
3. Merge k sorted arrays using a heap.
4. Implement a running median algorithm using two heaps.
5. Design a data structure that supports insert, delete-min, delete-max, find-min, and find-max in O(log n) time.
6. Implement the heapify operation recursively and iteratively. Compare their performance.
7. Given a nearly sorted array, where each element is at most k positions away from its sorted position, sort the array efficiently.

Remember to analyze the time and space complexity of your solutions!

## Conclusion

Heaps and priority queues are fundamental data structures with numerous applications in computer science and real-world problems. Understanding their properties, operations, and use cases is crucial for designing efficient algorithms and solving complex problems. As you work through the practice problems, focus on understanding the trade-offs between different approaches and how heaps can be applied in various scenarios.