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