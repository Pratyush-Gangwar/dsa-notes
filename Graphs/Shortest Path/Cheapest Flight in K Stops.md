## Code
```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<int> curr(n, INT_MAX);
        curr[src] = 0;
        
        for(int i = 1; i <= k + 1; i++) {
            vector<int> prev = curr;

            for(auto& edge : flights) {
                int u = edge[0];
                int v = edge[1];
                int w = edge[2];
                
                if (prev[u] != INT_MAX && prev[u] + w < curr[v]) {
                    curr[v] = prev[u] + w;
                }
            }

        }
        
        return (curr[dst] == INT_MAX ? -1 : curr[dst]);
    }
};
```

We can solve **Cheapest Flights Within K Stops** using Bellman-Ford because of the following invariant:

> After the `i`-th iteration, we know the cheapest path from `src` to every vertex using at most `i` edges.

A route with at most `k` stops uses at most:

```
k + 1
```

flights (edges). Therefore, we only need information about paths using at most `k + 1` edges, so we run the outer loop exactly `k + 1` times. If `dst` is still unreachable, we return `-1`.

However, we must use the **DP version** of Bellman-Ford (with separate `prev` and `curr` arrays) rather than the standard in-place version. The standard Bellman-Ford implementation can propagate information through multiple edges during a single iteration, which breaks the invariant that iteration `i` corresponds exactly to paths using at most `i` edges.

## Complexities
Time: `O( (k + 1) * V )`
Space: `O( V )`