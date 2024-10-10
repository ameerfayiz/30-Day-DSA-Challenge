## Table of Contents
1. Introduction to Graphs
2. Graph Terminology
3. Types of Graphs
4. Graph Representations
   4.1 Adjacency Matrix
   4.2 Adjacency List
5. Graph Traversals
   5.1 Breadth-First Search (BFS)
   5.2 Depth-First Search (DFS)
6. Comparison of BFS and DFS
7. Applications of Graph Algorithms
8. Practice Problems

## 1. Introduction to Graphs

A graph is a non-linear data structure consisting of vertices (or nodes) and edges that connect these vertices. Graphs are used to represent networks of communication, data organization, computational devices, flow of computation, etc.

## 2. Graph Terminology

- **Vertex (Node)**: A fundamental unit of which graphs are formed.
- **Edge**: A connection between two vertices.
- **Adjacent Vertices**: Two vertices are adjacent if there's an edge connecting them.
- **Path**: A sequence of vertices where each adjacent pair is connected by an edge.
- **Cycle**: A path that starts and ends at the same vertex.
- **Degree**: The number of edges connected to a vertex.
- **In-degree**: The number of edges pointing to the vertex (in directed graphs).
- **Out-degree**: The number of edges pointing from the vertex (in directed graphs).

## 3. Types of Graphs

1. **Undirected Graph**: Edges have no direction.
2. **Directed Graph (Digraph)**: Edges have directions.
3. **Weighted Graph**: Edges have weights/costs associated with them.
4. **Unweighted Graph**: Edges have no weights.
5. **Connected Graph**: There's a path between every pair of vertices.
6. **Disconnected Graph**: Some vertices cannot be reached from others.
7. **Cyclic Graph**: The graph has at least one cycle.
8. **Acyclic Graph**: The graph has no cycles.
9. **Tree**: A connected acyclic undirected graph.
10. **Directed Acyclic Graph (DAG)**: A directed graph with no cycles.

## 4. Graph Representations

### 4.1 Adjacency Matrix

An adjacency matrix is a 2D array of size V x V where V is the number of vertices in the graph. Let the 2D array be adj[][], a slot adj[i][j] = 1 indicates that there is an edge from vertex i to vertex j.

#### Advantages:
1. Space-efficient for dense graphs.
2. Edge lookup is O(1).
3. Convenient to implement.

#### Disadvantages:
1. Consumes more space O(V^2) for sparse graphs.
2. Adding a vertex is O(V^2) time.

#### Implementation:

```python
class Graph:
    def __init__(self, vertices):
        self.V = vertices
        self.graph = [[0 for column in range(vertices)] 
                      for row in range(vertices)]
    
    def add_edge(self, u, v):
        self.graph[u][v] = 1
        self.graph[v][u] = 1  # for undirected graph
```

### 4.2 Adjacency List

An adjacency list is an array of lists. The size of the array is equal to the number of vertices. Let the array be array[]. An entry array[i] represents the list of vertices adjacent to the ith vertex.

#### Advantages:
1. Space-efficient for sparse graphs.
2. Adding a vertex is easier.

#### Disadvantages:
1. Edge lookup is O(V) for worst case.
2. Not cache-friendly like adjacency matrix.

#### Implementation:

```python
from collections import defaultdict

class Graph:
    def __init__(self):
        self.graph = defaultdict(list)
    
    def add_edge(self, u, v):
        self.graph[u].append(v)
        self.graph[v].append(u)  # for undirected graph
```

## 5. Graph Traversals

Graph traversal means visiting every vertex in the graph. The two most common traversal algorithms are Breadth-First Search (BFS) and Depth-First Search (DFS).

### 5.1 Breadth-First Search (BFS)

BFS is an algorithm for traversing or searching tree or graph data structures. It starts at a chosen root vertex and explores all of the neighbor vertices at the present depth prior to moving on to the vertices at the next depth level.

#### Algorithm:
1. Choose a starting vertex and add it to a queue.
2. Remove the first vertex from the queue and mark it as visited.
3. Add all unvisited neighbors of this vertex to the queue.
4. Repeat steps 2-3 until the queue is empty.

#### Implementation:

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)

    while queue:
        vertex = queue.popleft()
        print(vertex, end=" ")

        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

#### Time Complexity: O(V + E) where V is the number of vertices and E is the number of edges.
#### Space Complexity: O(V)

### 5.2 Depth-First Search (DFS)

DFS is an algorithm for traversing or searching tree or graph data structures. The algorithm starts at a chosen root vertex and explores as far as possible along each branch before backtracking.

#### Algorithm:
1. Choose a starting vertex and add it to a stack.
2. While the stack is not empty:
   a. Pop a vertex from the stack and mark it as visited.
   b. Push all unvisited neighbors of this vertex to the stack.
3. Repeat step 2 until the stack is empty.

#### Implementation:

```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(start)
    print(start, end=" ")

    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

#### Time Complexity: O(V + E) where V is the number of vertices and E is the number of edges.
#### Space Complexity: O(V) in the worst case (for a skewed graph).

## 6. Comparison of BFS and DFS

| Aspect                   | BFS                                   | DFS                                   |
|--------------------------|---------------------------------------|---------------------------------------|
| Data Structure           | Queue                                 | Stack (or recursion)                  |
| Implementation           | Typically iterative                   | Can be recursive or iterative         |
| Space Complexity         | O(V)                                  | O(V) worst case, O(h) average         |
| Completeness             | Complete (finds all solutions)        | Not complete (may not find all)       |
| Optimality               | Optimal for unweighted graphs         | Not optimal                           |
| Use Case                 | Shortest path, closest nodes          | Topological sorting, cycle detection  |
| Graph Type Suitability   | Better for shallow, wide graphs       | Better for deep, narrow graphs        |

## 7. Applications of Graph Algorithms

1. **Social Networks**: Friend suggestions, connection paths.
2. **Web Crawling**: Indexing web pages for search engines.
3. **GPS Navigation**: Finding shortest paths between locations.
4. **Recommendation Systems**: Suggesting products or content.
5. **Network Routing**: Efficient data transmission in computer networks.
6. **Puzzle Solving**: Solving mazes, 8-puzzle problem, etc.
7. **Garbage Collection**: Mark-and-sweep algorithm in programming languages.
8. **Dependency Resolution**: Package managers in software development.

## 8. Practice Problems

1. Implement both adjacency matrix and adjacency list representations for a given graph.
2. Write a function to check if a graph is connected using BFS or DFS.
3. Implement a function to find the shortest path between two vertices using BFS.
4. Use DFS to detect cycles in a graph.
5. Implement a function to count the number of connected components in an undirected graph.
6. Write a program to determine if a graph is bipartite using BFS or DFS.
7. Implement topological sorting of a Directed Acyclic Graph (DAG) using DFS.
8. Write a function to find the strongly connected components in a directed graph using Kosaraju's algorithm.

Remember to analyze the time and space complexity of your solutions!

## Conclusion

Understanding graph basics, including their representations and traversal algorithms, is crucial for solving many real-world problems. These concepts form the foundation for more advanced graph algorithms and data structures. As you practice and gain more experience, you'll find that many complex problems can be modeled and solved efficiently using graphs.