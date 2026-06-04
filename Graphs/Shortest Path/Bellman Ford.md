## Why?
Djisktra doesn't work for single-source-shortest-path if graph has negative weights.
Bellman Ford works despite negative weights (but can't handle negative cycles).

## Recurrence
```
d_i(v)
=
min(
    d_{i-1}(v),
    min over edges (u,v)
        d_{i-1}(u) + w(u,v)
)
```

## DP Bellman Ford
```cpp
vector<int> dist(n, INF);
dist[src] = 0;

for (int i = 1; i <= k + 1; i++) {
    vector<int> prev = dist;

    for (auto& edge : edges) {
        int u = edge[0];
        int v = edge[1];
        int w = edge[2];

        if (prev[u] != INF) {
            dist[v] = min(dist[v], prev[u] + w);
        }
    }
}
```

Invariant:
> After iteration `i`, `dist[v]` is the shortest path from `src` to `v` using at most `i` edges.

### Outer and Inner Loops
Outer loop = number of edges allowed in the shortest path (invariant)
Inner loop = consider every possible last edge (recurrence)

## In-Place Bellman Ford
This is the textbook shortest-path algorithm.

```cpp
vector<int> dist(n, INF);
dist[src] = 0;

for (int i = 1; i <= n - 1; i++) {
    for (auto& edge : edges) {
        int u = edge[0];
        int v = edge[1];
        int w = edge[2];

        if (dist[u] != INF) {
            dist[v] = min(dist[v], dist[u] + w);
        }
    }
}
```

The invariant is weaker:
> After the k-th pass, every shortest path that uses at most k edges has been found.

Notice that it doesn't say the array only contains shortest paths using at most `k` edges. The distance array may accidentally discover some longer paths early. That's okay. 


## How does the In-Place algorithm differ?
Information discovered earlier in the same pass can be reused later in the same pass.

For example, consider this graph with source = `0`.
```
0 -> 1 (1)
1 -> 2 (1)
```

Suppose edges are processed in that order.

Initially:
```
dist = [0, INF, INF]
```

During the **first pass**:
```
0->1: dist[1] = 1
1->2: dist[2] = 2
```

After just one pass, you've already found a path using **2 edges**. So it is definitely **not true** that after pass 1 you've only found paths using ≤1 edge.

## How does the DP variant fix it?
The DP version fixes it by enforcing a very specific rule:

> During iteration `i`, you're only allowed to extend paths that were already known after iteration `i-1`.

That's exactly what the `prev` array does.

```
prev = d_{i-1}
dist = d_i
```

Note, we compare `prev[u] + w` to `curr[v]` since 
- initially, `curr[v]` is just `prev[u]` (covers the `min( d_{i-1}(v), ...)` part of recurrence)
- `curr[v]` represents any updates that come from `prev[u] + w` (covers the `min( ..., min over edges (u,v) d_{i-1}(u) + w(u,v))` part of recurrence)

## Why != INT_MAX in the code?
Numerical: Adding `w` to `dist[u] = INT_MAX` will overflow.
Theoretical: No point relaxing an edge which starts with an unreachable vertex. 

## Shortest Paths and Negative Cycles
Suppose after `V-1` iterations, an extra relaxation succeeds.
Then for some vertex `v`:
```
d_V(v) < d_{V-1}(v)
```

Since it uses `V` edges, it visits `V+1` vertices.
By the pigeonhole principle, some vertex repeats.

Therefore the path must look like:
```
src -> ... -> A
              -> ...
              -> ...
              -> A   (same A again)
              -> ...
              -> v
```

The repeated occurrence of `A` forms a cycle:
```
A -> ... -> A
```

Now suppose this cycle has nonnegative weight:
```
weight(A -> ... -> A) >= 0
```

The cost of the entire path is:
```
cost(src -> first A)
+
weight(A -> ... -> A)
+
cost(second A -> v)
```

If we remove the cycle, we obtain:
```
src -> ... -> A
              -> ...
              -> v
```

Because the cycle has nonnegative weight, removing it cannot increase the path cost.

Therefore the new path has:
- at most `V-1` edges (we removed at least one edge),
- cost less than or equal to the original path.

Hence
```
d_{V-1}(v) <= d_V(v).
```

But we started with
```
d_V(v) < d_{V-1}(v).
```

Contradiction.

Therefore
```
weight(A -> ... -> A) < 0.
```

So the path contains a negative cycle.

## Why only V - 1 iterations of the outer loop?
Consider what could happen if we ran one more iteration.

### Case 1: No relaxation succeeds
Then:

```
d_V(v) = d_{V-1}(v)
```

for every vertex `v`.

Allowing an extra edge didn't improve anything, so we've already found the final shortest distances.

### Case 2: Some relaxation succeeds
Then for some vertex `v`:

```
d_V(v) < d_{V-1}(v)
```

But we've already proved that this implies a reachable negative cycle.

In that case, a shortest path isn't even well-defined, because you can keep going around the negative cycle and make the path cost arbitrarily small.

## Conclusion Theorem: Shortest Paths Need At Most `V−1` Edges (if no reachable negative cycle exists)


