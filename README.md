# Practical - 07

## Implementation of Making Change Problem using Dynamic Programming

### Aim
To find the minimum number of coins required to make a target amount using dynamic programming and analyze its complexity.

### Algorithm
1. Initialize dp[0] = 0 and dp[1...A] = ∞.
2. For each amount x from 1 to A, try every coin denomination.
3. If coin ≤ x and dp[x-coin] is finite, update dp[x] = min(dp[x], dp[x-coin] + 1).
4. Return dp[A], or -1 if it is still infinite.

### C++ Program
```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>
using namespace std;

int minCoins(const vector<int>& coins, int amount) {
    vector<int> dp(amount + 1, INT_MAX);
    dp[0] = 0;

    for (int x = 1; x <= amount; x++) {
        for (int coin : coins) {
            if (coin <= x && dp[x - coin] != INT_MAX) {
                dp[x] = min(dp[x], dp[x - coin] + 1);
            }
        }
    }
    return dp[amount] == INT_MAX ? -1 : dp[amount];
}

int main() {
    vector<int> coins = {1, 3, 4};
    int amount = 6;
    cout << minCoins(coins, amount);
}
```

### Output
```text
2
```

### Step-by-Step Analysis
- dp[0] = 0
- dp[1] = 1 using {1}
- dp[2] = 2 using {1,1}
- dp[3] = 1 using {3}
- dp[4] = 1 using {4}
- dp[5] = 2 using {4,1}
- dp[6] = 2 using {3,3}

### Time and Space Complexity
- Time Complexity: Θ(Am), where A is the target amount and m is the number of coin denominations.
- Space Complexity: Θ(A).

### Result
The Making Change problem was solved successfully using dynamic programming, and the minimum number of coins for amount 6 is 2.
