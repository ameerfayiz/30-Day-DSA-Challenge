## Table of Contents
1. Introduction to Advanced Graph Algorithms
2. Shortest Path Algorithms
   2.1 Dijkstra's Algorithm
   2.2 Bellman-Ford Algorithm
3. Minimum Spanning Tree Algorithms
   3.1 Prim's Algorithm
   3.2 Kruskal's Algorithm
4. Comparison of Algorithms
5. Practical Applications
6. Practice Problems

## 1. Introduction to Advanced Graph Algorithms

Advanced graph algorithms are essential tools for solving complex problems in computer science and real-world applications. They build upon basic graph concepts and traversal techniques to solve specific problems efficiently.

Key Concepts:
- Weighted Graphs: Graphs where edges have associated costs or weights.
- Shortest Path: The path between two vertices with the minimum total edge weight.
- Spanning Tree: A tree that includes all vertices of a graph with the minimum possible number of edges.
- Minimum Spanning Tree (MST): A spanning tree with the minimum total edge weight.

## 2. Shortest Path Algorithms

Shortest path algorithms find the path with the minimum total weight between two vertices in a weighted graph.

### 2.1 Dijkstra's Algorithm

Dijkstra's algorithm finds the shortest path from a source vertex to all other vertices in a weighted graph with non-negative edge weights.

#### Algorithm:
1. Initialize distances to all vertices as infinite and distance to the source as 0.
2. Create a set of unvisited vertices.
3. For the current vertex, consider all its unvisited neighbors and calculate their tentative distances.
4. When done considering all neighbors, mark the current vertex as visited and remove it from the unvisited set.
5. If the destination vertex has been marked visited, we're done.
6. Otherwise, select the unvisited vertex with the smallest tentative distance, and repeat from step 3.

#### Implementation in Python:
```python
import heapq

def dijkstra(graph, source):
    distances = {vertex: float('infinity') for vertex in graph}
    distances[source] = 0
    pq = [(0, source)]
    
    while pq:
        current_distance, current_vertex = heapq.heappop(pq)
        
        if current_distance > distances[current_vertex]:
            continue
        
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances
```

#### Time Complexity:
- O((V + E) log V) with a binary heap implementation
- O(V^2) with a simple array implementation

#### Space Complexity:
- O(V)

#### Advantages:
1. Efficient for sparse graphs
2. Works well with non-negative edge weights
3. Finds the shortest path to all vertices from the source

#### Disadvantages:
1. Doesn't work with negative edge weights
2. Can be slower for dense graphs

### 2.2 Bellman-Ford Algorithm

The Bellman-Ford algorithm finds the shortest paths from a source vertex to all other vertices in a weighted graph, even with negative edge weights.

#### Algorithm:
1. Initialize distances to all vertices as infinite and distance to the source as 0.
2. Repeat V-1 times:
   - For each edge (u, v) with weight w:
     - If distance[u] + w < distance[v], update distance[v] = distance[u] + w
3. Check for negative weight cycles:
   - For each edge (u, v) with weight w:
     - If distance[u] + w < distance[v], a negative weight cycle exists

#### Implementation in Python:
```python
def bellman_ford(graph, source):
    distances = {vertex: float('infinity') for vertex in graph}
    distances[source] = 0
    
    for _ in range(len(graph) - 1):
        for u in graph:
            for v, weight in graph[u].items():
                if distances[u] + weight < distances[v]:
                    distances[v] = distances[u] + weight
    
    # Check for negative weight cycles
    for u in graph:
        for v, weight in graph[u].items():
            if distances[u] + weight < distances[v]:
                raise ValueError("Graph contains a negative weight cycle")
    
    return distances
```

#### Time Complexity:
- O(VE)

#### Space Complexity:
- O(V)

#### Advantages:
1. Works with negative edge weights
2. Can detect negative weight cycles
3. Simpler implementation than Dijkstra's algorithm

#### Disadvantages:
1. Slower than Dijkstra's algorithm for most graphs
2. Not suitable for large, sparse graphs

## 3. Minimum Spanning Tree Algorithms

Minimum Spanning Tree (MST) algorithms find a subset of edges that connect all vertices in a weighted, undirected graph with the minimum total edge weight.

### 3.1 Prim's Algorithm

Prim's algorithm builds the MST by always adding the lowest-weight edge that connects a vertex in the tree to a vertex outside the tree.

#### Algorithm:
1. Start with an arbitrary vertex as a single-vertex tree.
2. Repeat until all vertices are in the tree:
   - Choose the minimum weight edge that connects a vertex in the tree to a vertex outside the tree.
   - Add the chosen edge and vertex to the tree.

#### Implementation in Python:
```python
import heapq

def prim(graph):
    start_vertex = next(iter(graph))
    mst = []
    visited = set([start_vertex])
    edges = [(weight, start_vertex, to) for to, weight in graph[start_vertex].items()]
    heapq.heapify(edges)
    
    while edges:
        weight, frm, to = heapq.heappop(edges)
        if to not in visited:
            visited.add(to)
            mst.append((frm, to, weight))
            for next_to, next_weight in graph[to].items():
                if next_to not in visited:
                    heapq.heappush(edges, (next_weight, to, next_to))
    
    return mst
```

#### Time Complexity:
- O((V + E) log V) with a binary heap implementation
- O(V^2) with a simple array implementation

#### Space Complexity:
- O(V)

#### Advantages:
1. Efficient for dense graphs
2. Builds the MST incrementally

#### Disadvantages:
1. Can be slower for sparse graphs compared to Kruskal's algorithm

### 3.2 Kruskal's Algorithm

Kruskal's algorithm builds the MST by adding edges in order of increasing weight, skipping edges that would create a cycle.

#### Algorithm:
1. Sort all edges in ascending order of weight.
2. Initialize a forest where each vertex is a separate tree.
3. For each edge in the sorted list:
   - If the edge connects two different trees, add it to the MST and merge the trees.
   - If the edge would create a cycle, skip it.
4. Continue until V-1 edges have been added to the MST.

#### Implementation in Python:
```python
class UnionFind:
    def __init__(self, vertices):
        self.parent = {v: v for v in vertices}
        self.rank = {v: 0 for v in vertices}
    
    def find(self, item):
        if self.parent[item] != item:
            self.parent[item] = self.find(self.parent[item])
        return self.parent[item]
    
    def union(self, x, y):
        xroot = self.find(x)
        yroot = self.find(y)
        if self.rank[xroot] < self.rank[yroot]:
            self.parent[xroot] = yroot
        elif self.rank[xroot] > self.rank[yroot]:
            self.parent[yroot] = xroot
        else:
            self.parent[yroot] = xroot
            self.rank[xroot] += 1

def kruskal(graph):
    edges = [(weight, u, v) for u in graph for v, weight in graph[u].items()]
    edges.sort()
    vertices = list(graph.keys())
    uf = UnionFind(vertices)
    mst = []
    
    for weight, u, v in edges:
        if uf.find(u) != uf.find(v):
            uf.union(u, v)
            mst.append((u, v, weight))
    
    return mst
```

#### Time Complexity:
- O(E log E) or O(E log V)

#### Space Complexity:
- O(V)

#### Advantages:
1. Efficient for sparse graphs
2. Simple to implement with a good union-find data structure

#### Disadvantages:
1. Requires sorting all edges, which can be slow for very large graphs

## 4. Comparison of Algorithms

| Algorithm     | Time Complexity    | Space Complexity | Weighted Edges | Negative Weights | Sparse Graphs | Dense Graphs |
|---------------|---------------------|-------------------|----------------|-------------------|---------------|--------------|
| Dijkstra      | O((V + E) log V)    | O(V)              | Yes            | No                | Efficient     | Less Efficient |
| Bellman-Ford  | O(VE)               | O(V)              | Yes            | Yes               | Less Efficient| Less Efficient |
| Prim          | O((V + E) log V)    | O(V)              | Yes            | Yes               | Less Efficient| Efficient     |
| Kruskal       | O(E log E)          | O(V)              | Yes            | Yes               | Efficient     | Less Efficient |

## 5. Practical Applications

1. Shortest Path Algorithms:
   - GPS and navigation systems
   - Network routing protocols
   - Logistics and transportation planning
   - Social network analysis (finding degrees of separation)

2. Minimum Spanning Tree Algorithms:
   - Network design (e.g., laying cable for computer networks)
   - Cluster analysis in data mining
   - Image segmentation in computer vision
   - Approximation algorithms for traveling salesman problem

## 6. Practice Problems

1. Implement Dijkstra's algorithm to find the shortest path between two specific vertices.
2. Use the Bellman-Ford algorithm to detect a negative weight cycle in a graph.
3. Implement Prim's algorithm using both an adjacency matrix and an adjacency list representation of a graph. Compare the running times.
4. Use Kruskal's algorithm to find the Maximum Spanning Tree of a graph.
5. Given a weighted graph representing a network of cities, find the minimum cost to connect all cities such that there is a path between every pair of cities.
6. Implement a modified version of Dijkstra's algorithm to find the path with the maximum probability of success (product of probabilities on edges) between two vertices.

Remember to analyze the time and space complexity of your solutions!

## Conclusion

Understanding these advanced graph algorithms is crucial for solving complex network and optimization problems efficiently. As you progress in your algorithmic journey, you'll find these concepts forming the foundation for many real-world applications in computer science, operations research, and network analysis.