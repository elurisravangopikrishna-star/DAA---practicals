# Practical - 09

## Implementation of Prim's Algorithm

### Aim
To implement Prim's algorithm to find a Minimum Spanning Tree (MST) of a connected weighted undirected graph and analyze its complexity.

### Algorithm
1. Start from vertex 0 and set its key value to 0; set all other key values to infinity.
2. Repeatedly select the unvisited vertex with the smallest key value.
3. Mark the selected vertex as used and update the key and parent of each adjacent unvisited vertex when a smaller edge is found.
4. After selecting V - 1 edges, print the parent edges and calculate their total cost.

### C++ Program
```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

int main() {
    const int V = 5;
    int g[V][V] = {
        {0,2,0,6,0},
        {2,0,3,8,5},
        {0,3,0,0,7},
        {6,8,0,0,9},
        {0,5,7,9,0}
    };

    vector<int> key(V, INT_MAX), parent(V, -1);
    vector<bool> used(V, false);
    key[0] = 0;

    for (int count = 0; count < V - 1; count++) {
        int u = -1;
        for (int v = 0; v < V; v++)
            if (!used[v] && (u == -1 || key[v] < key[u])) u = v;

        used[u] = true;
        for (int v = 0; v < V; v++)
            if (g[u][v] && !used[v] && g[u][v] < key[v]) {
                parent[v] = u;
                key[v] = g[u][v];
            }
    }

    int cost = 0;
    for (int v = 1; v < V; v++) {
        cout << parent[v] << " - " << v << " : " << g[v][parent[v]] << endl;
        cost += g[v][parent[v]];
    }
    cout << "MST cost = " << cost;
}
```

### Output
```text
0 - 1 : 2
1 - 2 : 3
0 - 3 : 6
1 - 4 : 5
MST cost = 16
```

### Step-by-Step Analysis
- Let V be the number of vertices. This implementation uses an adjacency matrix and a linear scan to select the minimum-key unvisited vertex.
- The outer loop executes V - 1 times; selecting the next minimum-key vertex scans V vertices: Θ(V²).
- Scanning the selected vertex's adjacency-matrix row also costs Θ(V²) overall.
- The matrix requires Θ(V²) storage; key, parent and used arrays require Θ(V).

### Time and Space Complexity
- Time Complexity (matrix implementation): Θ(V²).
- Auxiliary Space excluding graph: Θ(V).
- Graph storage using matrix: Θ(V²).

### Result
Prim's algorithm produced an MST of total cost 16 for the sample graph.
