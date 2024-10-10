## Table of Contents
1. Introduction to Stacks and Queues
2. Stacks
   2.1. Stack Operations
   2.2. Stack Implementation
   2.3. Applications of Stacks
   2.4. Time and Space Complexity
3. Queues
   3.1. Queue Operations
   3.2. Queue Implementation
   3.3. Applications of Queues
   3.4. Time and Space Complexity
4. Variations of Queues
   4.1. Circular Queue
   4.2. Double-Ended Queue (Deque)
   4.3. Priority Queue
5. Introduction to Monotonic Stack/Queue
   5.1. Monotonic Stack
   5.2. Monotonic Queue
6. Practice Problems

## 1. Introduction to Stacks and Queues

Stacks and queues are fundamental data structures in computer science. They are both linear data structures that follow specific protocols for adding and removing elements.

- **Stack**: Follows the Last-In-First-Out (LIFO) principle.
- **Queue**: Follows the First-In-First-Out (FIFO) principle.

These data structures are used in various algorithms and have numerous real-world applications.

## 2. Stacks

A stack is an abstract data type that serves as a collection of elements, with two main operations:
- Push: Adds an element to the top of the stack.
- Pop: Removes the most recently added element from the top of the stack.

### 2.1. Stack Operations

1. **Push**: Add an element to the top of the stack.
2. **Pop**: Remove and return the top element from the stack.
3. **Peek** or **Top**: Return the top element without removing it.
4. **isEmpty**: Check if the stack is empty.
5. **Size**: Return the number of elements in the stack.

### 2.2. Stack Implementation

Stacks can be implemented using arrays or linked lists. Here's a simple implementation using a Python list:

```python
class Stack:
    def __init__(self):
        self.items = []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        return None

    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        return None

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)
```

### 2.3. Applications of Stacks

1. **Function Call Management**: Used in implementing function calls and recursion in programming languages.
2. **Expression Evaluation**: Used to evaluate arithmetic expressions, including infix to postfix conversion.
3. **Undo Mechanism**: Used in text editors and other applications to implement undo functionality.
4. **Backtracking Algorithms**: Used in maze-solving algorithms, game-playing algorithms, etc.
5. **Parsing**: Used in compilers and calculators for parsing expressions.
6. **Browser History**: Used to implement the back button functionality in web browsers.

### 2.4. Time and Space Complexity

For a stack implemented using a dynamic array:

- Push: O(1) amortized
- Pop: O(1)
- Peek: O(1)
- isEmpty: O(1)
- Space Complexity: O(n), where n is the number of elements in the stack

## 3. Queues

A queue is an abstract data type that serves as a collection of elements, with two main operations:
- Enqueue: Adds an element to the rear of the queue.
- Dequeue: Removes an element from the front of the queue.

### 3.1. Queue Operations

1. **Enqueue**: Add an element to the rear of the queue.
2. **Dequeue**: Remove and return the front element from the queue.
3. **Front**: Return the front element without removing it.
4. **isEmpty**: Check if the queue is empty.
5. **Size**: Return the number of elements in the queue.

### 3.2. Queue Implementation

Queues can be implemented using arrays or linked lists. Here's a simple implementation using a Python list:

```python
class Queue:
    def __init__(self):
        self.items = []

    def enqueue(self, item):
        self.items.append(item)

    def dequeue(self):
        if not self.is_empty():
            return self.items.pop(0)
        return None

    def front(self):
        if not self.is_empty():
            return self.items[0]
        return None

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)
```

### 3.3. Applications of Queues

1. **Task Scheduling**: Used in operating systems for process scheduling.
2. **Breadth-First Search**: Used in graph algorithms for level-order traversal.
3. **Buffering**: Used in various scenarios like print spooling, data transfer, etc.
4. **Message Queues**: Used in computer networks for handling message traffic.
5. **Synchronization**: Used in multi-threaded applications for synchronization between threads.
6. **Customer Service Systems**: Used in call centers and customer support applications.

### 3.4. Time and Space Complexity

For a queue implemented using a dynamic array:

- Enqueue: O(1) amortized
- Dequeue: O(n) (due to shifting elements)
- Front: O(1)
- isEmpty: O(1)
- Space Complexity: O(n), where n is the number of elements in the queue

Note: The dequeue operation can be optimized to O(1) by using a circular buffer or a linked list implementation.

## 4. Variations of Queues

### 4.1. Circular Queue

A circular queue is a variation of a queue where the last element is connected to the first element, forming a circle. It's also known as a ring buffer.

Key features:
- Efficient memory utilization
- Constant time complexity for both enqueue and dequeue operations

### 4.2. Double-Ended Queue (Deque)

A deque (pronounced "deck") is a queue that allows insertion and deletion at both ends.

Key operations:
- insertFront(): Add an element to the front
- insertRear(): Add an element to the rear
- deleteFront(): Remove an element from the front
- deleteRear(): Remove an element from the rear

### 4.3. Priority Queue

A priority queue is a type of queue where elements have associated priorities. Elements with higher priorities are served before elements with lower priorities.

Key features:
- Elements are dequeued based on their priority
- Can be implemented using a heap data structure for efficient operations

## 5. Introduction to Monotonic Stack/Queue

Monotonic stacks and queues are variations of the standard stack and queue data structures that maintain a monotonic order (either increasing or decreasing) of their elements.

### 5.1. Monotonic Stack

A monotonic stack is a stack that maintains its elements in a monotonic order (either strictly increasing or strictly decreasing).

Key features:
- Used to solve problems involving finding the nearest greater/smaller element
- Helpful in problems involving histograms or temperature spans

Example implementation (monotonic increasing stack):

```python
def next_greater_element(arr):
    n = len(arr)
    result = [-1] * n
    stack = []

    for i in range(n - 1, -1, -1):
        while stack and stack[-1] <= arr[i]:
            stack.pop()
        if stack:
            result[i] = stack[-1]
        stack.append(arr[i])

    return result
```

### 5.2. Monotonic Queue

A monotonic queue is similar to a monotonic stack but maintains the FIFO property of a queue while also keeping elements in a monotonic order.

Key features:
- Used in sliding window problems
- Helpful in maintaining maximum/minimum elements in a range

Example implementation (monotonic decreasing queue for sliding window maximum):

```python
from collections import deque

def max_sliding_window(nums, k):
    result = []
    window = deque()

    for i, num in enumerate(nums):
        while window and window[0] <= i - k:
            window.popleft()
        while window and nums[window[-1]] < num:
            window.pop()
        window.append(i)
        if i >= k - 1:
            result.append(nums[window[0]])

    return result
```

## 6. Practice Problems

1. Implement a stack that supports push, pop, top, and retrieving the minimum element in constant time.
2. Implement a queue using two stacks.
3. Design a stack that supports push, pop, top, and retrieving the maximum element in constant time.
4. Implement a circular queue.
5. Solve the "Next Greater Element" problem using a monotonic stack.
6. Implement a sliding window maximum using a monotonic queue.
7. Design a data structure that supports adding new words and finding if a string matches any previously added string with one typo.
8. Implement an LRU (Least Recently Used) cache.
9. Solve the "Trapping Rain Water" problem using a monotonic stack.
10. Implement a basic calculator to evaluate simple expressions.

Remember to analyze the time and space complexity of your solutions!

## Conclusion

Stacks and queues are fundamental data structures that form the building blocks of many advanced algorithms and data structures. Understanding their properties, operations, and applications is crucial for solving a wide range of programming problems efficiently. The variations like monotonic stacks and queues provide powerful tools for solving specific types of problems, particularly those involving ranges or sequences.

As you practice implementing these data structures and solving related problems, you'll develop a deeper understanding of their strengths and use cases. This knowledge will be invaluable as you progress to more complex algorithms and data structures.