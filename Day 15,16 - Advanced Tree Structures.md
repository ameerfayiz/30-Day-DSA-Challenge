## Table of Contents
1. Introduction to Advanced Tree Structures
2. AVL Trees
3. Red-Black Trees
4. B-Trees
5. Comparison of Tree Structures
6. Practical Applications
7. Practice Problems and Exercises

## 1. Introduction to Advanced Tree Structures

Advanced tree structures are self-balancing data structures designed to maintain balance and ensure efficient operations (insertion, deletion, and search) even in worst-case scenarios. These structures are crucial for maintaining performance in applications that require frequent updates and searches on large datasets.

Key concepts:
- Self-balancing
- Tree rotations
- Height or color properties
- Guaranteed logarithmic time complexity for basic operations

## 2. AVL Trees

AVL trees, named after their inventors Adelson-Velsky and Landis, are self-balancing binary search trees.

### Properties:
1. For each node, the heights of its left and right subtrees differ by at most 1.
2. Balance factor = Height(Left Subtree) - Height(Right Subtree)
3. Balance factor must be -1, 0, or 1 for all nodes.

### Operations:

#### 2.1 Insertion
1. Perform standard BST insertion
2. Update heights of ancestors
3. Check balance factor and perform rotations if necessary

#### 2.2 Rotation Types
1. Left Rotation
2. Right Rotation
3. Left-Right Rotation
4. Right-Left Rotation

```python
class AVLNode:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1

class AVLTree:
    def __init__(self):
        self.root = None
    
    def height(self, node):
        return node.height if node else 0
    
    def balance_factor(self, node):
        return self.height(node.left) - self.height(node.right)
    
    def update_height(self, node):
        node.height = 1 + max(self.height(node.left), self.height(node.right))
    
    def rotate_right(self, y):
        x = y.left
        T2 = x.right
        x.right = y
        y.left = T2
        self.update_height(y)
        self.update_height(x)
        return x
    
    def rotate_left(self, x):
        y = x.right
        T2 = y.left
        y.left = x
        x.right = T2
        self.update_height(x)
        self.update_height(y)
        return y
    
    def insert(self, root, key):
        if not root:
            return AVLNode(key)
        if key < root.key:
            root.left = self.insert(root.left, key)
        else:
            root.right = self.insert(root.right, key)
        
        self.update_height(root)
        balance = self.balance_factor(root)
        
        # Left Heavy
        if balance > 1:
            if key < root.left.key:
                return self.rotate_right(root)
            else:
                root.left = self.rotate_left(root.left)
                return self.rotate_right(root)
        
        # Right Heavy
        if balance < -1:
            if key > root.right.key:
                return self.rotate_left(root)
            else:
                root.right = self.rotate_right(root.right)
                return self.rotate_left(root)
        
        return root
```

#### 2.3 Deletion
1. Perform standard BST deletion
2. Update heights of ancestors
3. Check balance factor and perform rotations if necessary

### Time Complexity:
- Search: O(log n)
- Insertion: O(log n)
- Deletion: O(log n)

### Space Complexity:
- O(n) for storing n nodes

### Advantages:
1. Guaranteed O(log n) time complexity for basic operations
2. Suitable for applications requiring frequent insertions and deletions

### Disadvantages:
1. Extra space for storing height information
2. Complex implementation compared to simple BST

## 3. Red-Black Trees

Red-Black trees are self-balancing binary search trees with an extra bit of information per node: its color (red or black).

### Properties:
1. Every node is either red or black.
2. The root is always black.
3. Every leaf (NIL) is black.
4. If a node is red, then both its children are black.
5. For each node, all simple paths from the node to descendant leaves contain the same number of black nodes.

### Operations:

#### 3.1 Insertion
1. Perform standard BST insertion
2. Color the new node red
3. Perform recoloring and rotations to maintain Red-Black properties

#### 3.2 Fixing Violations
1. Recoloring
2. Rotations (similar to AVL trees)

```python
class RBNode:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.parent = None
        self.color = 1  # 1 for red, 0 for black

class RedBlackTree:
    def __init__(self):
        self.TNULL = RBNode(0)
        self.TNULL.color = 0
        self.root = self.TNULL
    
    def insert(self, key):
        node = RBNode(key)
        node.left = self.TNULL
        node.right = self.TNULL
        
        y = None
        x = self.root
        
        while x != self.TNULL:
            y = x
            if node.key < x.key:
                x = x.left
            else:
                x = x.right
        
        node.parent = y
        if y == None:
            self.root = node
        elif node.key < y.key:
            y.left = node
        else:
            y.right = node
        
        if node.parent == None:
            node.color = 0
            return
        
        if node.parent.parent == None:
            return
        
        self.fix_insert(node)
    
    def fix_insert(self, k):
        while k.parent.color == 1:
            if k.parent == k.parent.parent.right:
                u = k.parent.parent.left
                if u.color == 1:
                    u.color = 0
                    k.parent.color = 0
                    k.parent.parent.color = 1
                    k = k.parent.parent
                else:
                    if k == k.parent.left:
                        k = k.parent
                        self.right_rotate(k)
                    k.parent.color = 0
                    k.parent.parent.color = 1
                    self.left_rotate(k.parent.parent)
            else:
                u = k.parent.parent.right
                if u.color == 1:
                    u.color = 0
                    k.parent.color = 0
                    k.parent.parent.color = 1
                    k = k.parent.parent
                else:
                    if k == k.parent.right:
                        k = k.parent
                        self.left_rotate(k)
                    k.parent.color = 0
                    k.parent.parent.color = 1
                    self.right_rotate(k.parent.parent)
            if k == self.root:
                break
        self.root.color = 0
    
    def left_rotate(self, x):
        y = x.right
        x.right = y.left
        if y.left != self.TNULL:
            y.left.parent = x
        y.parent = x.parent
        if x.parent == None:
            self.root = y
        elif x == x.parent.left:
            x.parent.left = y
        else:
            x.parent.right = y
        y.left = x
        x.parent = y
    
    def right_rotate(self, x):
        y = x.left
        x.left = y.right
        if y.right != self.TNULL:
            y.right.parent = x
        y.parent = x.parent
        if x.parent == None:
            self.root = y
        elif x == x.parent.right:
            x.parent.right = y
        else:
            x.parent.left = y
        y.right = x
        x.parent = y
```

#### 3.3 Deletion
1. Perform standard BST deletion
2. Perform recoloring and rotations to maintain Red-Black properties

### Time Complexity:
- Search: O(log n)
- Insertion: O(log n)
- Deletion: O(log n)

### Space Complexity:
- O(n) for storing n nodes

### Advantages:
1. Guaranteed O(log n) time complexity for basic operations
2. More efficient rebalancing compared to AVL trees (fewer rotations)

### Disadvantages:
1. Complex implementation
2. Extra space for storing color information

## 4. B-Trees

B-Trees are self-balancing search trees designed to work efficiently on disk-based storage systems. Unlike AVL and Red-Black trees, B-Trees are not binary trees; each node can have more than two children.

### Properties:
1. All leaves are at the same level.
2. A non-leaf node with k children contains k-1 keys.
3. Each node (except root) must contain at least t-1 keys, where t is the minimum degree of the B-tree.
4. All nodes (including root) may contain at most 2t-1 keys.
5. Number of children of a node is equal to its number of keys plus one.

### Operations:

#### 4.1 Search
Similar to binary search tree, but with multiple keys in each node.

#### 4.2 Insertion
1. If the node is not full, insert the key into the appropriate position.
2. If the node is full:
   a. Split the node
   b. Move the median key to the parent
   c. Repeat the process for the parent if necessary

#### 4.3 Deletion
1. If the key is in a leaf node, simply remove it.
2. If the key is in an internal node:
   a. Replace the key with its in-order predecessor or successor
   b. Delete the predecessor or successor from the appropriate child
3. If the node becomes under-full, borrow keys from siblings or merge nodes.

### Time Complexity:
- Search: O(log n)
- Insertion: O(log n)
- Deletion: O(log n)

### Space Complexity:
- O(n) for storing n keys

### Advantages:
1. Efficient for systems that read and write large blocks of data
2. Minimizes disk I/O operations
3. Suitable for databases and file systems

### Disadvantages:
1. Complex implementation
2. Not as efficient as other self-balancing trees for in-memory operations

## 5. Comparison of Tree Structures

| Aspect               | AVL Trees                   | Red-Black Trees            | B-Trees                    |
|----------------------|-----------------------------|----------------------------|----------------------------|
| Balance Condition    | Strict (balance factor ≤ 1) | Relaxed (black height)     | Flexible (node occupancy)  |
| Height               | Always balanced             | Approximately balanced     | Always balanced            |
| Insertion Efficiency | More rotations              | Fewer rotations            | Efficient for large nodes  |
| Deletion Efficiency  | More rotations              | Fewer rotations            | Efficient for large nodes  |
| Memory Usage         | Extra space for height      | Extra space for color      | Efficient for large data   |
| Use Case             | Frequent searches           | Frequent insertions/deletions | Disk-based storage systems |

## 6. Practical Applications

### AVL Trees:
1. Database indexing
2. Implementing sets and maps in C++ STL
3. File system organization

### Red-Black Trees:
1. Java TreeMap and TreeSet implementations
2. Linux kernel's Completely Fair Scheduler
3. Implementing associative arrays

### B-Trees:
1. File systems (e.g., NTFS, HFS+)
2. Database indexing (e.g., MySQL, PostgreSQL)
3. Key-value stores (e.g., MongoDB, Couchbase)

## 7. Practice Problems and Exercises

1. Implement an AVL tree and perform insertions and deletions.
2. Implement a Red-Black tree and perform insertions and deletions.
3. Compare the performance of AVL and Red-Black trees for a large number of random insertions and deletions.
4. Implement a simple B-Tree with a fixed order and perform basic operations.
5. Design a data structure that combines properties of AVL and B-Trees for efficient in-memory and on-disk operations.
6. Implement a self-balancing binary search tree that uses randomization instead of strict balancing rules.

For each problem, analyze the time and space complexity of your solutions and compare them with the theoretical bounds discussed in the study material.

## Conclusion

Advanced tree structures like AVL trees, Red-Black trees, and B-Trees are crucial for maintaining efficient operations in large-scale applications. Understanding these structures and their trade-offs is essential for choosing the right data structure for specific use cases and optimizing performance in real-world scenarios.