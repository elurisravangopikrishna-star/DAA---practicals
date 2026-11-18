# Practical - 12

## Implementation of Travelling Salesman Problem

### Aim
To solve the Travelling Salesman Problem (TSP) using dynamic programming with bitmasking and analyze its exponential state space.

### Algorithm
1. Use dp[mask][u] to store the minimum cost to start at vertex 0, visit exactly the cities represented by mask, and finish at city u.
2. Initialize dp[1][0] = 0 and all other states to infinity.
3. For each reachable state, try every city v not already included in mask and update the new state using the travel cost from u to v.
4. After all cities are visited, add the return cost to city 0 and choose the minimum total tour cost.

### C++ Program
```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>
using namespace std;

int main() {
    vector<vector<int>> cost = {
        {0,10,15,20},
        {10,0,35,25},
        {15,35,0,30},
        {20,25,30,0}
    };

    int n = cost.size();
    int N = 1 << n;
    const int INF = 1000000000;
    vector<vector<int>> dp(N, vector<int>(n, INF));
    dp[1][0] = 0;

    for (int mask = 1; mask < N; mask++) {
        for (int u = 0; u < n; u++) {
            if (!(mask & (1 << u)) || dp[mask][u] == INF) continue;
            for (int v = 0; v < n; v++) {
                if (mask & (1 << v)) continue;
                int nextMask = mask | (1 << v);
                dp[nextMask][v] =
                    min(dp[nextMask][v], dp[mask][u] + cost[u][v]);
            }
        }
    }

    int ans = INF;
    int full = N - 1;
    for (int u = 1; u < n; u++)
        ans = min(ans, dp[full][u] + cost[u][0]);

    cout << "Minimum tour cost = " << ans;
}
```

### Output
```text
Minimum tour cost = 80
```

### Step-by-Step Analysis
- Let n be the number of cities.
- A bitmask has 2^n possible subsets of cities.
- For each subset mask, the DP stores a value for each possible current city u: number of DP states = n · 2^n.
- Each state may try up to n unvisited next cities: T(n) = O(n · 2^n · n) = O(n² · 2^n).
- The DP table contains n · 2^n entries. This is better than checking all n! tours by brute force, but it is still exponential.

### Time and Space Complexity
- Time Complexity: O(n² · 2^n).
- Space Complexity: O(n · 2^n).

### Result
The dynamic-programming TSP implementation found a minimum tour cost of 80 for the sample graph.
