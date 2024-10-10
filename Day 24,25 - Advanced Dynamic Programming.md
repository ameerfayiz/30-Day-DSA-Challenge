## Table of Contents
1. Introduction to Advanced Dynamic Programming
2. Longest Common Subsequence (LCS)
   2.1 Problem Definition
   2.2 Recursive Approach
   2.3 Dynamic Programming Approach
   2.4 Space Optimization
   2.5 Variations and Applications
3. Knapsack Problems
   3.1 0/1 Knapsack
      3.1.1 Problem Definition
      3.1.2 Recursive Approach
      3.1.3 Dynamic Programming Approach
      3.1.4 Space Optimization
   3.2 Unbounded Knapsack
      3.2.1 Problem Definition
      3.2.2 Dynamic Programming Approach
   3.3 Variations and Applications
4. Practice Problems
5. Conclusion

## 1. Introduction to Advanced Dynamic Programming

Dynamic Programming (DP) is a method for solving complex problems by breaking them down into simpler subproblems. It is a powerful technique that combines the correctness of complete search and the efficiency of greedy algorithms.

Key aspects of Dynamic Programming:
- Optimal Substructure: An optimal solution to the problem contains optimal solutions to subproblems.
- Overlapping Subproblems: The same subproblems are solved multiple times.

In this section, we'll explore two classic DP problems: Longest Common Subsequence (LCS) and Knapsack problems.

## 2. Longest Common Subsequence (LCS)

### 2.1 Problem Definition

The Longest Common Subsequence problem is to find the longest subsequence present in given two sequences in the same order, i.e., find the longest sequence which can be obtained from both the original sequences by deleting some elements without changing the order of the remaining elements.

Example:
- Input: 
  - X = "ABCDGH"
  - Y = "AEDFHR"
- Output: "ADH" (length 3)

### 2.2 Recursive Approach

The recursive approach to solve LCS:

```python
def lcs_recursive(X, Y, m, n):
    if m == 0 or n == 0:
        return 0
    elif X[m-1] == Y[n-1]:
        return 1 + lcs_recursive(X, Y, m-1, n-1)
    else:
        return max(lcs_recursive(X, Y, m, n-1), lcs_recursive(X, Y, m-1, n))
```

Time Complexity: O(2^(m+n))
Space Complexity: O(m+n) due to the recursion stack

### 2.3 Dynamic Programming Approach

We can solve this problem efficiently using DP:

```python
def lcs_dp(X, Y):
    m, n = len(X), len(Y)
    L = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if X[i-1] == Y[j-1]:
                L[i][j] = L[i-1][j-1] + 1
            else:
                L[i][j] = max(L[i-1][j], L[i][j-1])
    
    return L[m][n]
```

Time Complexity: O(mn)
Space Complexity: O(mn)

### 2.4 Space Optimization

We can optimize the space usage to O(min(m,n)):

```python
def lcs_dp_optimized(X, Y):
    m, n = len(X), len(Y)
    if m < n:
        X, Y = Y, X
        m, n = n, m
    
    current = [0] * (n + 1)
    
    for i in range(1, m + 1):
        prev = current[:]
        for j in range(1, n + 1):
            if X[i-1] == Y[j-1]:
                current[j] = prev[j-1] + 1
            else:
                current[j] = max(current[j-1], prev[j])
    
    return current[n]
```

Time Complexity: O(mn)
Space Complexity: O(min(m,n))

### 2.5 Variations and Applications

1. Longest Common Substring: Find the longest string that is a substring of two or more strings.
2. Shortest Common Supersequence: Find the shortest supersequence Z such that both X and Y are subsequences of Z.
3. Diff Utility: Used in version control systems to show differences between files.
4. Biological Sequence Alignment: Used in bioinformatics to align DNA, RNA, or protein sequences.

## 3. Knapsack Problems

### 3.1 0/1 Knapsack

#### 3.1.1 Problem Definition

Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. In the 0/1 Knapsack problem, we are not allowed to break items. We either take the whole item or don't take it.

Example:
- Input: 
  - values[] = {60, 100, 120}
  - weights[] = {10, 20, 30}
  - W = 50
- Output: 220

#### 3.1.2 Recursive Approach

```python
def knapsack_recursive(W, wt, val, n):
    if n == 0 or W == 0:
        return 0
    if wt[n-1] > W:
        return knapsack_recursive(W, wt, val, n-1)
    else:
        return max(
            val[n-1] + knapsack_recursive(W-wt[n-1], wt, val, n-1),
            knapsack_recursive(W, wt, val, n-1)
        )
```

Time Complexity: O(2^n)
Space Complexity: O(n) due to the recursion stack

#### 3.1.3 Dynamic Programming Approach

```python
def knapsack_dp(W, wt, val, n):
    K = [[0 for _ in range(W + 1)] for _ in range(n + 1)]
    
    for i in range(n + 1):
        for w in range(W + 1):
            if i == 0 or w == 0:
                K[i][w] = 0
            elif wt[i-1] <= w:
                K[i][w] = max(val[i-1] + K[i-1][w-wt[i-1]], K[i-1][w])
            else:
                K[i][w] = K[i-1][w]
    
    return K[n][W]
```

Time Complexity: O(nW)
Space Complexity: O(nW)

#### 3.1.4 Space Optimization

We can optimize the space usage to O(W):

```python
def knapsack_dp_optimized(W, wt, val, n):
    dp = [0 for _ in range(W + 1)]
    
    for i in range(n):
        for w in range(W, wt[i]-1, -1):
            dp[w] = max(dp[w], val[i] + dp[w-wt[i]])
    
    return dp[W]
```

Time Complexity: O(nW)
Space Complexity: O(W)

### 3.2 Unbounded Knapsack

#### 3.2.1 Problem Definition

In the Unbounded Knapsack problem, we are allowed to use unlimited number of instances of an item.

Example:
- Input: 
  - values[] = {10, 30, 20}
  - weights[] = {5, 10, 15}
  - W = 100
- Output: 300

#### 3.2.2 Dynamic Programming Approach

```python
def unbounded_knapsack(W, wt, val, n):
    dp = [0 for _ in range(W + 1)]
    
    for w in range(W + 1):
        for i in range(n):
            if wt[i] <= w:
                dp[w] = max(dp[w], dp[w-wt[i]] + val[i])
    
    return dp[W]
```

Time Complexity: O(nW)
Space Complexity: O(W)

### 3.3 Variations and Applications

1. Subset Sum Problem: Determine if there is a subset of the given set with sum equal to given sum.
2. Partition Problem: Determine whether a given set can be partitioned into two subsets such that the sum of elements in both subsets is the same.
3. Coin Change Problem: Given a set of coin denominations, find the minimum number of coins needed to make a given amount.
4. Cutting Rod Problem: Given a rod of length n inches and prices of pieces of size smaller than n, find the maximum value obtainable by cutting up the rod and selling the pieces.

## 4. Practice Problems

1. Implement a function to print the Longest Common Subsequence.
2. Solve the Longest Increasing Subsequence problem using DP.
3. Implement the Coin Change problem (both for counting number of ways and finding minimum number of coins).
4. Solve the Matrix Chain Multiplication problem using DP.
5. Implement the Edit Distance problem using DP.

## 5. Conclusion

Advanced Dynamic Programming problems like LCS and Knapsack are fundamental to many real-world applications and form the basis for solving more complex optimization problems. Understanding these problems and their solutions will greatly enhance your problem-solving skills and prepare you for tackling more advanced algorithmic challenges.

Remember, the key to mastering DP is practice. Try to identify the optimal substructure and overlapping subproblems in each new problem you encounter. With time and practice, you'll develop an intuition for when and how to apply DP techniques effectively.