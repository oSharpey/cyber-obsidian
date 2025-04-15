# Breadth First Search
## How it works:
1. Put the starting vertex into the queue.
2. For each vertex taken from the queue, visit the vertex, then put all unvisited adjacent vertices into the queue.
3. Continue as long as there are vertices in the queue.
## Time and Space Complexities:
- **Time Complexity:**  
    _O(V + E)_ where _V_ is the number of vertices and _E_ is the number of edges.
- **Space Complexity:**  
    _O(V)_ because in the worst case, all vertices may be stored in the queue.

## Implementation
### With Class
```python
class Graph:
    def __init__(self, size):
        self.adj_matrix = [[0] * size for _ in range(size)]
        self.size = size
        self.vertex_data = [''] * size  

    def add_edge(self, u, v):
        if 0 <= u < self.size and 0 <= v < self.size:
            self.adj_matrix[u][v] = 1
            self.adj_matrix[v][u] = 1

    def add_vertex_data(self, vertex, data):
        if 0 <= vertex < self.size:
            self.vertex_data[vertex] = data

    def print_graph(self):
        print("Adjacency Matrix:")
        for row in self.adj_matrix:
            print(' '.join(map(str, row)))
        print("\nVertex Data:")
        for vertex, data in enumerate(self.vertex_data):
            print(f"Vertex {vertex}: {data}")
            
    def bfs(self, start_vertex_data):
        queue = [self.vertex_data.index(start_vertex_data)]
        visited = [False] * self.size
        visited[queue[0]] = True
                
        while queue:
            current_vertex = queue.pop(0)
            print(self.vertex_data[current_vertex], end=' ')
            
            for i in range(self.size):
                if self.adj_matrix[current_vertex][i] == 1 and not visited[i]:
                    queue.append(i)
                    visited[i] = True

g = Graph(7)

g.add_vertex_data(0, 'A')
g.add_vertex_data(1, 'B')
g.add_vertex_data(2, 'C')
g.add_vertex_data(3, 'D')
g.add_vertex_data(4, 'E')
g.add_vertex_data(5, 'F')
g.add_vertex_data(6, 'G')

g.add_edge(3, 0)  # D - A
g.add_edge(0, 2)  # A - C
g.add_edge(0, 3)  # A - D
g.add_edge(0, 4)  # A - E
g.add_edge(4, 2)  # E - C
g.add_edge(2, 5)  # C - F
g.add_edge(2, 1)  # C - B
g.add_edge(2, 6)  # C - G
g.add_edge(1, 5)  # B - F

g.print_graph()

print("\nBreadth First Search starting from vertex D:")
g.bfs('D')
```
### No Class
```python
from collections import deque

def bfs(graph, start):
    visited = []
    queue = deque([start])
    seen = set([start])
    
    while queue:
        vertex = queue.popleft()
        visited.append(vertex)
        for neighbor in graph.get(vertex, []):
            if neighbor not in seen:
                seen.add(neighbor)
                queue.append(neighbor)
    return visited

# Example usage:
if __name__ == '__main__':
    graph = {
        'A': ['B', 'C'],
        'B': ['A', 'D', 'E'],
        'C': ['A', 'F'],
        'D': ['B'],
        'E': ['B', 'F'],
        'F': ['C', 'E']
    }
    print("BFS Traversal:", bfs(graph, 'A'))
```

## Adversarial for BFS:
Graphs with an extremely high branching factor (wide graphs) can quickly use up memory, as many nodes are enqueued simultaneously.
- **Mitigation:**  
    Limiting the depth or using iterative deepening can help if only a limited search depth is required.


# Depth First Search
## How it works
- Depth-first search commonly referred to as DFS and breadth-first search are two algorithms used to search through trees and graphs.
- DFS starts at the root node, and goes as far down one branch as possible, all the way to the end.
- Once we hit the deepest point, we come back to an un-visited branch and go down that one.
## Time and Space Complexities:
- **Time Complexity:**  
    _O(V + E)_ since every vertex and edge is explored.
- **Space Complexity:**  
    _O(V)_ in the worst case, mainly due to the recursion stack (if the graph is deep) or explicit stack if using an iterative approach.
## Adversarial for DFS:
Graphs with very deep paths or skewed trees can lead to deep recursion (or large stack sizes), possibly causing stack overflow errors.
- **Mitigation:**  
    Use an iterative implementation with an explicit stack, or incorporate recursion depth control/iterative deepening.
## Implementation
```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(start)
    visited_order = [start]
    
    for neighbor in graph.get(start, []):
        if neighbor not in visited:
            visited_order.extend(dfs(graph, neighbor, visited))
    
    return visited_order

# Example usage:
if __name__ == '__main__':
    graph = {
        'A': ['B', 'C'],
        'B': ['D', 'E'],
        'C': ['F'],
        'D': [],
        'E': ['F'],
        'F': []
    }
    print("DFS Traversal:", dfs(graph, 'A'))
```

```python
class Graph:
    def __init__(self, size):
        self.adj_matrix = [[0] * size for _ in range(size)]
        self.size = size
        self.vertex_data = [''] * size  

    def add_edge(self, u, v):
        if 0 <= u < self.size and 0 <= v < self.size:
            self.adj_matrix[u][v] = 1
            self.adj_matrix[v][u] = 1

    def add_vertex_data(self, vertex, data):
        if 0 <= vertex < self.size:
            self.vertex_data[vertex] = data

    def print_graph(self):
        print("Adjacency Matrix:")
        for row in self.adj_matrix:
            print(' '.join(map(str, row)))
        print("\nVertex Data:")
        for vertex, data in enumerate(self.vertex_data):
            print(f"Vertex {vertex}: {data}")
            
    def dfs_util(self, v, visited):
        visited[v] = True
        print(self.vertex_data[v], end=' ')

        for i in range(self.size):
            if self.adj_matrix[v][i] == 1 and not visited[i]:
                self.dfs_util(i, visited)

    def dfs(self, start_vertex_data):
        visited = [False] * self.size
        start_vertex = self.vertex_data.index(start_vertex_data)
        self.dfs_util(start_vertex, visited)

g = Graph(7)

g.add_vertex_data(0, 'A')
g.add_vertex_data(1, 'B')
g.add_vertex_data(2, 'C')
g.add_vertex_data(3, 'D')
g.add_vertex_data(4, 'E')
g.add_vertex_data(5, 'F')
g.add_vertex_data(6, 'G')

g.add_edge(3, 0)  # D - A
g.add_edge(0, 2)  # A - C
g.add_edge(0, 3)  # A - D
g.add_edge(0, 4)  # A - E
g.add_edge(4, 2)  # E - C
g.add_edge(2, 5)  # C - F
g.add_edge(2, 1)  # C - B
g.add_edge(2, 6)  # C - G
g.add_edge(1, 5)  # B - F

g.print_graph()

print("\nDepth First Search starting from vertex D:")
g.dfs('D')
```

# Dijkstras
## How it works
1. Set initial distances for all vertices: 0 for the source vertex, and infinity for all the other.
2. Choose the unvisited vertex with the shortest distance from the start to be the current vertex. So the algorithm will always start with the source as the current vertex.
3. For each of the current vertex's unvisited neighbor vertices, calculate the distance from the source and update the distance if the new, calculated, distance is lower.
4. We are now done with the current vertex, so we mark it as visited. A visited vertex is not checked again.
5. Go back to step 2 to choose a new current vertex, and keep repeating these steps until all vertices are visited.
6. In the end we are left with the shortest path from the source vertex to every other vertex in the graph.

## Implementation
```python
class Graph:
    def __init__(self, size):
        self.adj_matrix = [[0] * size for _ in range(size)]
        self.size = size
        self.vertex_data = [''] * size

    def add_edge(self, u, v, weight):
        if 0 <= u < self.size and 0 <= v < self.size:
            self.adj_matrix[u][v] = weight
            self.adj_matrix[v][u] = weight  # For undirected graph

    def add_vertex_data(self, vertex, data):
        if 0 <= vertex < self.size:
            self.vertex_data[vertex] = data

    def dijkstra(self, start_vertex_data):
        start_vertex = self.vertex_data.index(start_vertex_data)
        distances = [float('inf')] * self.size
        distances[start_vertex] = 0
        visited = [False] * self.size

        for _ in range(self.size):
            min_distance = float('inf')
            u = None
            for i in range(self.size):
                if not visited[i] and distances[i] < min_distance:
                    min_distance = distances[i]
                    u = i

            if u is None:
                break

            visited[u] = True

            for v in range(self.size):
                if self.adj_matrix[u][v] != 0 and not visited[v]:
                    alt = distances[u] + self.adj_matrix[u][v]
                    if alt < distances[v]:
                        distances[v] = alt

        return distances

g = Graph(7)

g.add_vertex_data(0, 'A')
g.add_vertex_data(1, 'B')
g.add_vertex_data(2, 'C')
g.add_vertex_data(3, 'D')
g.add_vertex_data(4, 'E')
g.add_vertex_data(5, 'F')
g.add_vertex_data(6, 'G')

g.add_edge(3, 0, 4)  # D - A, weight 5
g.add_edge(3, 4, 2)  # D - E, weight 2
g.add_edge(0, 2, 3)  # A - C, weight 3
g.add_edge(0, 4, 4)  # A - E, weight 4
g.add_edge(4, 2, 4)  # E - C, weight 4
g.add_edge(4, 6, 5)  # E - G, weight 5
g.add_edge(2, 5, 5)  # C - F, weight 5
g.add_edge(2, 1, 2)  # C - B, weight 2
g.add_edge(1, 5, 2)  # B - F, weight 2
g.add_edge(6, 5, 5)  # G - F, weight 5

# Dijkstra's algorithm from D to all vertices
print("\nDijkstra's Algorithm starting from vertex D:")
distances = g.dijkstra('D')
for i, d in enumerate(distances):
    print(f"Distance from D to {g.vertex_data[i]}: {d}")
```


# MST
- The Minimum Spanning Tree (MST) is the collection of edges required to connect all vertices in an undirected graph, with the minimum total edge weight.
- It is called a Minimum Spanning **Tree**, because it is a connected, acyclic, undirected graph, which is the definition of a tree data structure.
- In the real world, finding the Minimum Spanning Tree can help us find the most effective way to connect houses to the internet or to the electrical grid, or it can help us finding the fastest route to deliver packages.

- **Dense Graphs:**  
    Dense graphs may slow down both algorithms due to the large number of edges. Efficient sorting (Kruskal) or efficient priority queue operations (Prim) are critical.
- **Sparse Graphs:**  
    Both algorithms generally perform well on sparse graphs.
- **Mitigation Strategies:**
    - When using Kruskal's, a well-implemented union-find can mitigate cycle detection overhead.
    - When using Prim's, choosing the right data structure (e.g., Fibonacci heaps or optimized binary heaps) can improve performance.

|                                                                         | Prims Algorithm                              | Kruskals Algorithm                                            |
| ----------------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------------------------- |
| _Can it find MSTs (a Minimum Spanning Forest) in an unconnected graph?_ | No                                           | Yes                                                           |
| _How does it start?_                                                    | The MST grows from a randomly chosen vertex. | The first edge in the MST is the edge with lowest edge weight |
| _What time complexity does it have?_                                    | O(V^2), or O(E⋅logV) (Optimized)             | O(E⋅logE)                                                     |
## Kruskals
- The MST (or MSTs) found by Kruskal's algorithm is the collection of edges that connect all vertices (or as many as possible) with the minimum total edge weight.
- Kruskal's algorithm adds edges to the MST (or Minimum Spanning Forest), starting with the edges with the lowest edge weights.
- Edges that would create a cycle are not added to the MST. These are the red blinking lines in the animation above.
- Kruskal's algorithm checks all edges in the graph

### How it works
1. Sort the edges in the graph from the lowest to the highest edge weight.
2. For each edge, starting with the one with the lowest edge weight:
    1. Will this edge create a cycle in the current MST?
        - If no: Add the edge as an MST edge.
-