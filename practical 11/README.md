# Practical - 11

## Implementation of Floyd-Warshall Algorithm

### Aim
To implement the Floyd-Warshall algorithm for all-pairs shortest paths and derive its cubic time complexity.

### Algorithm
1. Create a distance matrix; use 0 for the diagonal and a large value INF for unreachable pairs.
2. Consider each vertex k as an intermediate vertex.
3. For every pair (i, j), update d[i][j] with the smaller of its current value and d[i][k] + d[k][j], when both parts are finite.
4. Print the final shortest-distance matrix, showing -1 for unreachable pairs.

### C++ Program
```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>
using namespace std;

int main() {
    const int INF = 1000000;
    vector<vector<int>> d = {
        {0,5,INF,10},
        {INF,0,3,INF},
        {INF,INF,0,1},
        {INF,INF,INF,0}
    };

    int V = d.size();
    for (int k = 0; k < V; k++)
        for (int i = 0; i < V; i++)
            for (int j = 0; j < V; j++)
                if (d[i][k] < INF && d[k][j] < INF)
                    d[i][j] = min(d[i][j], d[i][k] + d[k][j]);

    for (auto &row : d) {
        for (int x : row)
            cout << (x >= INF ? -1 : x) << "\t";
        cout << endl;
    }
}
```

### Output
```text
0	5	8	9
-1	0	3	4
-1	-1	0	1
-1	-1	-1	0
```

### Step-by-Step Analysis
- Let V be the number of vertices.
- The algorithm has three nested loops over k, i and j; each loop runs V times.
- The innermost operation performs constant-time arithmetic, comparison and assignment.
- T(V) = V · V · V · Θ(1) = Θ(V³).
- The distance table contains V² entries.

### Time and Space Complexity
- Time Complexity: Θ(V³).
- Space Complexity: Θ(V²).

### Result
Floyd-Warshall successfully computed the shortest distance between every ordered pair of vertices.
