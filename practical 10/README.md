# Practical - 10

## Implementation of Kruskal's Algorithm

### Aim
To implement Kruskal's algorithm using Disjoint Set Union (DSU) and analyze the cost of sorting and cycle detection.

### Algorithm
1. Represent each weighted edge by its two endpoints and weight.
2. Sort all edges in increasing order of weight.
3. Initialize a Disjoint Set Union (DSU) structure with each vertex in its own set.
4. Process edges in sorted order; add an edge only if its endpoints belong to different sets, then unite those sets.
5. Stop after selecting V - 1 edges and report the total MST cost.

### C++ Program
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Edge { int u, v, w; };

struct DSU {
    vector<int> p, rankv;
    DSU(int n): p(n), rankv(n, 0) {
        for (int i = 0; i < n; i++) p[i] = i;
    }
    int find(int x) {
        return p[x] == x ? x : p[x] = find(p[x]);
    }
    bool unite(int a, int b) {
        a = find(a);
        b = find(b);
        if (a == b) return false;
        if (rankv[a] < rankv[b]) swap(a, b);
        p[b] = a;
        if (rankv[a] == rankv[b]) rankv[a]++;
        return true;
    }
};

int main() {
    int V = 4;
    vector<Edge> edges = {
        {0,1,10}, {0,2,6}, {0,3,5}, {1,3,15}, {2,3,4}
    };

    sort(edges.begin(), edges.end(),
         [](Edge a, Edge b) { return a.w < b.w; });

    DSU dsu(V);
    int cost = 0;
    for (auto e : edges) {
        if (dsu.unite(e.u, e.v)) {
            cout << e.u << " - " << e.v << " : " << e.w << endl;
            cost += e.w;
        }
    }
    cout << "MST cost = " << cost;
}
```

### Output
```text
2 - 3 : 4
0 - 3 : 5
0 - 1 : 10
MST cost = 19
```

### Step-by-Step Analysis
- Let V be the number of vertices and E the number of edges.
- Sorting all E edges costs Θ(E log E).
- Each edge is considered at most once. With path compression and union by rank, DSU operations cost amortized O(α(V)).
- DSU processing cost = O(E α(V)); sorting dominates, so T(V,E) = Θ(E log E).

### Time and Space Complexity
- Time Complexity: Θ(E log E), dominated by sorting.
- DSU auxiliary storage: Θ(V).
- Edge-list storage: Θ(E).

### Result
Kruskal's algorithm successfully constructed the MST and avoided cycles using DSU.
