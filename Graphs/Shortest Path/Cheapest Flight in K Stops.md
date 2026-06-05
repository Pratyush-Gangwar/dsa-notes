## Bellman-Ford
### Code
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

### Complexities
Time: `O( (k + 1) * V )`
Space: `O( V )`

## BFS
### Code
```cpp
class Solution {
public:
    vector<vector<pair<int, int>>> makeAdjList(int n, vector<vector<int>>& flights) {
        vector<vector<pair<int, int>>> adjList(n);

        for(auto& flight : flights) {
            int u = flight[0];
            int v = flight[1];
            int w = flight[2];

            adjList[u].push_back( {v, w} );
        }

        return adjList;
    }

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<int> dist(n, INT_MAX);
        dist[src] = 0;

        vector<vector<pair<int, int>>> adjList = makeAdjList(n, flights);

        queue<pair<int, int>> q;
        q.push({src, 0});

        int level = 0;

        while (!q.empty()) {
            int levelSize = q.size();

            while (levelSize > 0) {
                auto [node, d] = q.front();
                q.pop();

                levelSize--;

                for(auto& np : adjList[node]) {
                    int neighbor = np.first;
                    int weight = np.second;

                    if (d + weight < dist[neighbor]) {
                        dist[neighbor] = d + weight;
                        q.push( {neighbor, dist[neighbor]} );
                    }
                }
            }

            if (level == k) break;
            level++;
        }

        return ( dist[dst] == INT_MAX ? -1 : dist[dst] );
    }
};
```

### Explanation
#### Why BFS and not Djisktra?
The question wants us to find the cheapest flight in K stops. This is naturally encompassed by BFS since it traverses the graph in level-order. On the other hand, Djisktra may end up using more than K stops since it does not have a built-in level-order constraint.

#### Why use the level-order formulation of BFS instead of regular BFS?
We need to keep track of the current level so that we can stop after K stops.

#### Why stop when level == K and not when K + 1?
TODO

#### Why do we push each node multiple times in the queue?
If the distance to a node improves, we need to recalculate the distance to all its neighbors. So, we must push it multiple times.

#### Why do we not have `visited` array like in regular BFS?
A node can be pushed into the queue multiple times if its distance improves. A `visited` array would not make this possible. 

#### Why do we store distance in the queue when we already have `dist` array?
The core tension: `dist[node]` is a single value that gets overwritten by the cheapest path seen so far. But in a stops-constrained problem, there can be **two meaningfully different paths** to the same node — one cheaper but stop-heavy, one costlier but stop-light. The `dist` array can only remember one of them, and it always picks the cheaper one.

This becomes a correctness issue when relaxing neighbors. If you use `dist[node]` to propagate forward, you're always propagating the cheapest path's cost — but that path may have consumed all K stops, making any further relaxation useless since the level guard will cut it off. The costlier, fewer-stop path — the one that actually has budget to reach `dst` — gets silently discarded because it lost the `dist[node]` overwrite battle.

Storing `d` in the queue preserves each path's own cost independently of `dist`. When you dequeue `{node, d}`, you propagate **this path's cost** forward, not the global minimum. The path that can actually reach `dst` survives even if it wasn't cheap enough to hold `dist[node]`

#### Example


#### Why do we not have `if (d != dist[node]) continue`
`d != dist[node]` means this path is costlier than the best known — but as established, costlier often means fewer stops remaining. Skipping it throws away the path that has budget left to reach `dst`, which is the entire reason you stored `d` in the queue in the first place.

### Complexities
TODO

Avg/Worst same as BF. 