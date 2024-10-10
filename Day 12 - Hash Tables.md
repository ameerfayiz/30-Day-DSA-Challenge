## Table of Contents
1. Introduction to Hash Tables
2. Hash Functions
3. Collision Resolution Techniques
4. Implementation of Hash Tables
5. Time and Space Complexity
6. Applications of Hash Tables
7. Advantages and Disadvantages
8. Practice Problems

## 1. Introduction to Hash Tables

Hash tables, also known as hash maps, are data structures that implement an associative array abstract data type, a structure that can map keys to values. They use a hash function to compute an index into an array of buckets or slots, from which the desired value can be found.

### Key Concepts:
- **Key**: The identifier used to access data in the hash table
- **Value**: The data associated with a key
- **Hash Function**: A function that converts keys into array indices
- **Bucket**: A slot in the hash table array that can store one or more key-value pairs
- **Load Factor**: The ratio of the number of stored elements to the size of the hash table

## 2. Hash Functions

A hash function is a function that can map data of arbitrary size to fixed-size values. In the context of hash tables, it's used to compute an index in the array where a value should be inserted or searched.

### Properties of a Good Hash Function:
1. **Deterministic**: The same input should always produce the same output
2. **Efficient**: It should be quick to compute
3. **Uniform Distribution**: It should map the expected inputs as evenly as possible over its output range
4. **Non-invertible**: It should be difficult to reconstruct the input from the output

### Common Hash Functions:

#### 2.1 Division Method
```python
def hash_division(key, table_size):
    return key % table_size
```

#### 2.2 Multiplication Method
```python
def hash_multiplication(key, table_size, A=0.6180339887):
    return int(table_size * ((key * A) % 1))
```

#### 2.3 Universal Hashing
```python
def universal_hash(key, a, b, p, m):
    return ((a * key + b) % p) % m
```

#### 2.4 String Hashing
```python
def string_hash(s, table_size):
    hash_value = 0
    for char in s:
        hash_value = (hash_value * 31 + ord(char)) % table_size
    return hash_value
```

### 2.5 Cryptographic Hash Functions
While not typically used for hash tables, it's worth mentioning cryptographic hash functions like SHA-256 or MD5. These are designed for security purposes and have additional properties like collision resistance and the avalanche effect.

## 3. Collision Resolution Techniques

Collisions occur when two different keys hash to the same index. There are several techniques to handle collisions:

### 3.1 Separate Chaining (Open Hashing)

In this method, each bucket is independent, and has some sort of list of entries with the same index. The time for hash table operations is the time to find the bucket (constant) plus the time for the list operation.

#### Implementation:
```python
class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None

class HashTable:
    def __init__(self, size):
        self.size = size
        self.table = [None] * size

    def _hash(self, key):
        return hash(key) % self.size

    def insert(self, key, value):
        index = self._hash(key)
        if self.table[index] is None:
            self.table[index] = Node(key, value)
        else:
            current = self.table[index]
            while current:
                if current.key == key:
                    current.value = value
                    return
                if current.next is None:
                    break
                current = current.next
            current.next = Node(key, value)

    def get(self, key):
        index = self._hash(key)
        current = self.table[index]
        while current:
            if current.key == key:
                return current.value
            current = current.next
        raise KeyError(key)
```

### 3.2 Open Addressing

In open addressing, all elements are stored in the hash table itself. When a collision occurs, we probe for next available slot.

#### 3.2.1 Linear Probing
```python
class HashTable:
    def __init__(self, size):
        self.size = size
        self.keys = [None] * size
        self.values = [None] * size

    def _hash(self, key):
        return hash(key) % self.size

    def insert(self, key, value):
        index = self._hash(key)
        while self.keys[index] is not None:
            if self.keys[index] == key:
                self.values[index] = value
                return
            index = (index + 1) % self.size
        self.keys[index] = key
        self.values[index] = value

    def get(self, key):
        index = self._hash(key)
        while self.keys[index] is not None:
            if self.keys[index] == key:
                return self.values[index]
            index = (index + 1) % self.size
        raise KeyError(key)
```

#### 3.2.2 Quadratic Probing
Similar to linear probing, but the interval between probes increases quadratically. This helps to avoid the clustering problem of linear probing.

```python
def insert(self, key, value):
    index = self._hash(key)
    i = 0
    while self.keys[index] is not None:
        if self.keys[index] == key:
            self.values[index] = value
            return
        i += 1
        index = (self._hash(key) + i*i) % self.size
    self.keys[index] = key
    self.values[index] = value
```

#### 3.2.3 Double Hashing
Uses a second hash function to determine the probe interval.

```python
def _hash2(self, key):
    return 1 + (hash(key) % (self.size - 1))

def insert(self, key, value):
    index = self._hash(key)
    if self.keys[index] is None:
        self.keys[index] = key
        self.values[index] = value
    else:
        i = 1
        while True:
            new_index = (index + i * self._hash2(key)) % self.size
            if self.keys[new_index] is None:
                self.keys[new_index] = key
                self.values[new_index] = value
                break
            i += 1
```

## 4. Implementation of Hash Tables

Most modern programming languages provide built-in hash table implementations:

- Python: `dict`
- Java: `HashMap`
- C++: `std::unordered_map`
- JavaScript: `Object` or `Map`

These implementations typically use a combination of the techniques we've discussed, optimized for general use cases.

## 5. Time and Space Complexity

### Time Complexity:
- Average case (assuming a good hash function and resolution strategy):
  - Insert: O(1)
  - Delete: O(1)
  - Search: O(1)
- Worst case (all keys collide):
  - Insert: O(n)
  - Delete: O(n)
  - Search: O(n)

### Space Complexity:
- O(n), where n is the number of key-value pairs stored

## 6. Applications of Hash Tables

1. Database indexing
2. Caches (e.g., web browser cache)
3. Symbol tables in compilers and interpreters
4. Password verification
5. Spell checkers
6. Unique data representation (e.g., removing duplicates)
7. Cryptography (e.g., digital signatures)

## 7. Advantages and Disadvantages

### Advantages:
1. Fast lookups: Ideal for dictionary-type storage
2. Flexible keys: Can use complex objects as keys
3. Constant-time performance for basic operations (average case)

### Disadvantages:
1. May have poor performance if many collisions occur
2. No ordering of keys
3. Not effective for small datasets due to overhead
4. Potential for clustering, especially with simple hash functions

## 8. Practice Problems

1. Implement a hash table using separate chaining that supports dynamic resizing.
2. Design a hash function for a custom object (e.g., a Person class with name and age).
3. Implement the LRU (Least Recently Used) cache data structure using a hash table.
4. Given an array of integers, find two numbers such that they add up to a specific target number. (Hint: Use a hash table for O(n) time complexity)
5. Implement a spell checker using a hash table.

## Conclusion

Hash tables are powerful data structures that offer constant-time average case performance for insert, delete, and search operations. Understanding hash functions and collision resolution techniques is crucial for implementing efficient and reliable hash tables. As you solve the practice problems, think about how hash tables can be applied to optimize solutions to various algorithmic problems.