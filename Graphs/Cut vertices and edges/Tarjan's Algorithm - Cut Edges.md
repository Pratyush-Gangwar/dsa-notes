## When is an edge a cut edge?

Not a bridge when `low[v] <= disc[u]`

That means `v`'s subtree has a back edge reaching `u` or higher — an alternate path exists, so `u→v` is redundant.

**The `==` case:** `low[v] == disc[u]` means the back edge reaches exactly `u`. The edge `u→v` is still redundant — `u` stays intact when you remove an edge, so that back edge still works.

Flip it: **bridge when `low[v] > disc[u]`** — no alternate path exists at all.

**Why cut vertices use `>=`:** Removing a _node_ kills the back edge's landing point too. So even if `low[v] == disc[u]`, that back edge dies with `u`. No savings — still a cut vertex.

## Code
```cpp
int cnt = 0;

void helper(int src, int parent, vector<int>& disc, vector<int>& low, vector<bool>& visited, vector<vector<int>>& adjList, vector<pair<int, int>>& ans) {
    disc[src] = cnt;
    low[src] = disc[src];
    cnt++;
    
    visited[src] = true;
    
    for(int neighbor : adjList[src]) {
        if (!visited[neighbor]) {
            helper(neighbor, src, disc, low, visited, adjList, c, d));
            
            low[src] = min(low[src], low[neighbor]);

            if (low[neighbor] > disc[src]) {
                ans.push_back( {src, neighbor} );
            }
        }
        
        else if (neighbor != parent) {
            low[src] = min(low[src], disc[neighbor]);
        }
        
    }
        
}
```

For cut edges, we don't really need a separate case for the root node. In case it has only one tree edge, that edge is a cut edge. In case
