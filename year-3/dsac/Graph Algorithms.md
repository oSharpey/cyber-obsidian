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
