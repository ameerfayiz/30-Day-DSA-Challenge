## Table of Contents
1. Introduction to String Matching Algorithms
2. Knuth-Morris-Pratt (KMP) Algorithm
   2.1 Overview
   2.2 The Prefix Function
   2.3 KMP Algorithm Implementation
   2.4 Time and Space Complexity
   2.5 Applications
3. Rabin-Karp Algorithm
   3.1 Overview
   3.2 Rolling Hash Function
   3.3 Rabin-Karp Algorithm Implementation
   3.4 Time and Space Complexity
   3.5 Applications
4. Comparison of KMP and Rabin-Karp
5. Practice Problems

## 1. Introduction to String Matching Algorithms

String matching is a fundamental problem in computer science, involving finding occurrences of a pattern string within a larger text string. Naive approaches can be inefficient for large texts, leading to the development of more sophisticated algorithms like KMP and Rabin-Karp.

## 2. Knuth-Morris-Pratt (KMP) Algorithm

### 2.1 Overview

The KMP algorithm, developed by Donald Knuth, James H. Morris, and Vaughan Pratt in 1977, is an efficient string matching algorithm. It utilizes the observation that when a mismatch occurs, the pattern itself embodies sufficient information to determine where the next match could begin, avoiding the need to reexamine previously matched characters.

### 2.2 The Prefix Function

The key to the KMP algorithm is the prefix function (also called the failure function or the pi function). This function encapsulates knowledge about the pattern to optimize the matching process.

Definition: For a string s of length n, the prefix function π is defined as:
π[i] = the length of the longest proper prefix of the substring s[0:i+1] which is also a suffix of this substring.

Example:
For the pattern "ABABAC":
π[0] = 0 (by definition)
π[1] = 0 (no proper prefix which is also a suffix)
π[2] = 1 ("A" is the longest proper prefix which is also a suffix of "ABA")
π[3] = 2 ("AB" is the longest proper prefix which is also a suffix of "ABAB")
π[4] = 3 ("ABA" is the longest proper prefix which is also a suffix of "ABABA")
π[5] = 0 (no proper prefix which is also a suffix of "ABABAC")

Implementation of the prefix function:

```python
def compute_prefix_function(pattern):
    n = len(pattern)
    π = [0] * n
    k = 0
    for q in range(1, n):
        while k > 0 and pattern[k] != pattern[q]:
            k = π[k-1]
        if pattern[k] == pattern[q]:
            k += 1
        π[q] = k
    return π
```

### 2.3 KMP Algorithm Implementation

```python
def kmp_search(text, pattern):
    n = len(text)
    m = len(pattern)
    π = compute_prefix_function(pattern)
    q = 0  # number of characters matched
    results = []
    for i in range(n):
        while q > 0 and pattern[q] != text[i]:
            q = π[q-1]  # next character does not match
        if pattern[q] == text[i]:
            q += 1  # next character matches
        if q == m:  # is all of pattern matched?
            results.append(i - m + 1)
            q = π[q-1]  # look for the next match
    return results
```

### 2.4 Time and Space Complexity

- Time Complexity: O(n + m), where n is the length of the text and m is the length of the pattern.
  - Computing the prefix function takes O(m) time.
  - The main search process takes O(n) time.
- Space Complexity: O(m) for storing the prefix function.

### 2.5 Applications

1. Text editors: For "find" functionality
2. Intrusion detection in computer networks
3. Computational biology: DNA sequence matching
4. Data compression algorithms

## 3. Rabin-Karp Algorithm

### 3.1 Overview

The Rabin-Karp algorithm, developed by Richard M. Karp and Michael O. Rabin in 1987, is a string-searching algorithm that uses hashing to find an exact match of a pattern string in a text. It is particularly effective when searching for multiple patterns simultaneously.

### 3.2 Rolling Hash Function

The key idea behind the Rabin-Karp algorithm is the use of a rolling hash function. This function allows us to compute the hash of the next substring in constant time by using the hash value of the previous substring.

A common choice for the hash function is the polynomial rolling hash:

H = c₁ * a^(k-1) + c₂ * a^(k-2) + ... + cₖ₋₁ * a + cₖ mod m

Where:
- c₁, c₂, ..., cₖ are the ASCII values of the characters
- a is a constant (usually a prime number)
- m is a large prime number
- k is the length of the substring

### 3.3 Rabin-Karp Algorithm Implementation

```python
def rabin_karp_search(text, pattern):
    n = len(text)
    m = len(pattern)
    d = 256  # number of characters in the input alphabet
    q = 101  # a prime number
    h = pow(d, m-1) % q
    p = 0  # hash value for pattern
    t = 0  # hash value for text
    results = []

    # Calculate the hash value of pattern and first window of text
    for i in range(m):
        p = (d*p + ord(pattern[i])) % q
        t = (d*t + ord(text[i])) % q

    # Slide the pattern over text one by one
    for i in range(n - m + 1):
        # Check the hash values of current window of text and pattern
        if p == t:
            # If the hash values match, check for characters one by one
            if text[i:i+m] == pattern:
                results.append(i)

        # Calculate hash value for next window of text
        if i < n - m:
            t = (d*(t - ord(text[i])*h) + ord(text[i+m])) % q
            if t < 0:
                t += q

    return results
```

### 3.4 Time and Space Complexity

- Average and Best Case Time Complexity: O(n + m), where n is the length of the text and m is the length of the pattern.
- Worst Case Time Complexity: O(nm) - occurs when there are many hash collisions.
- Space Complexity: O(1) - only a constant amount of extra space is used.

### 3.5 Applications

1. Plagiarism detection
2. Searching for patterns in DNA sequences
3. File systems: Detecting duplicate files based on content
4. Intrusion detection systems

## 4. Comparison of KMP and Rabin-Karp

| Aspect                    | KMP                                  | Rabin-Karp                           |
|---------------------------|--------------------------------------|--------------------------------------|
| Preprocessing             | Builds prefix function               | Computes hash of pattern             |
| Main Idea                 | Utilizes pattern structure           | Uses rolling hash function           |
| Best/Average Time         | O(n + m)                             | O(n + m)                             |
| Worst-case Time           | O(n + m)                             | O(nm)                                |
| Space Complexity          | O(m)                                 | O(1)                                 |
| Multiple Pattern Search   | Not efficient                        | Efficient                            |
| Sliding Window Technique  | No                                   | Yes                                  |
| False Positives           | No                                   | Possible (due to hash collisions)    |
| Best Use Case             | Single pattern search                | Multiple pattern search              |

## 5. Practice Problems

1. Implement the KMP algorithm to find all occurrences of a pattern in a text.
2. Modify the Rabin-Karp algorithm to search for multiple patterns simultaneously.
3. Use the KMP algorithm to find the longest prefix of a string that is also a suffix.
4. Implement a function that uses the Rabin-Karp algorithm to detect plagiarism between two documents.
5. Solve the "Repeated String Match" problem: Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.

Remember to analyze the time and space complexity of your solutions!

## Conclusion

Understanding these string matching algorithms is crucial for efficiently solving various problems involving text processing and pattern matching. Both KMP and Rabin-Karp offer significant improvements over naive string matching approaches, each with its own strengths and ideal use cases. As you solve more complex string-related problems, you'll find these algorithms forming the foundation for many advanced techniques in text processing and computational biology.