## Undirected
```cpp
count = 0;
for (v = 0; v < n; v++) {
    if (!visited[v]) {
        dfs(v);
        count++;
    }
}
```

## Directed
For directed graphs, you will need to define what connected components mean. Weakly connected components are connected by replacing all directed edges by undirected edges. Strongly connected components require a path b/w any two vertices in the component.

### An Example
Consider a simple graph `1 -> 0`. Let's run the above algorithm on it. 

Since `visited[0] = false`, the count is incremented and DFS is called. No other nodes are explored from `0` (there is no edge from `0` to `1`).

Since `visited[1] = false`, the count is incremented again. 

So, it gives the the incorrect number of weakly connected components (which is to be expected). By chance, it gives the correct number of strongly connected components (2 since every node can reach itself trivially). 

