## When is a vertex a cut vertex?
Say we want to check if node `u` is a cut vertex. Upon removing `u`, if all subgraphs containing neighbors are still connected to the rest of the graph, it is not a cut vertex. If even one subgraph containing a neighbor becomes disconnected from the rest of the graph, it is a cut vertex.

So, let us consider one of its neighbors `v`.  We can decompose the subgraph containing `v` into a DFS tree rooted at `v` (excluding `u` as a neighbor) and a set of back edges (remember the DFS tree + back edges gives the full graph). 

All tree edges stay in the DFS tree so they cannot connect `v`'s DFS tree to the rest of the graph. Back edges connect a node in the DFS tree to its ancestor. **If there exists at least one back edge from inside the subtree jumping strictly above `u`**, then `v`'s DFS tree is connected to the rest of the graph. 

## Cut Vertices
Take any tree-child `v` of `u`. From what we established: removing `u` disconnects `v`'s subtree iff there is no back edge from inside `v`'s subtree reaching strictly above `u`.

And `low[v]` is exactly the minimum `disc` reachable from `v`'s subtree. So:
- If `low[v] >= disc[u]` → the best escape from `v`'s subtree only reaches `u` or below. Removing `u` disconnects it. **`u` is a cut vertex.**
- If `low[v] < disc[u]` → there's a back edge escaping above `u`. `v`'s subtree stays connected.

## Code
```cpp
int cnt = 0;

void helper(int src, int parent, vector<int>& disc, vector<int>& low, vector<bool>& visited, vector<vector<int>>& adjList, vector<int>& ans) {
    disc[src] = cnt;
    low[src] = disc[src];
    cnt++;
    
    visited[src] = true;
    
    bool cut = false;
    int unvisited = 0;
    
    for(int neighbor : adjList[src]) {
        if (!visited[neighbor]) {
            helper(neighbor, src, disc, low, visited, adjList, ans);
            low[src] = min(low[src], low[neighbor]);
            unvisited++;
            
            if (low[neighbor] >= disc[src]) {
                cut = true;
            }
        }
        
        else if (neighbor != parent) {
            low[src] = min(low[src], disc[neighbor]);
        }
        
    }
    
    if ( (cut && parent != -1) || (parent == -1 && unvisited > 1) ) {
        ans.push_back(src);
    }
}
```

### disc, count
These are for tracking discovery time of nodes
### visited
Part of standard DFS. Used to distinguish tree and back edges here.
### low
`low[src]` is used by `src`'s DFS parent to determine if the parent is a cut vertex. 

It starts off as `disc[src]`.

If we encounter a tree edge (`visited[neighbor] == false`), we update `low[src]` with `low[neighbor]`. But only after calling `helper(neighbor, ...)` to ensure that `low[src]` is populated. 

If we encounter a back edge (`visited[neighbor] == true`), we update `low[src]` with `disc[neighbor]`.  

**Why don't we update `low[src]` with `low[neighbor]` if `(src, neighbor)` is a back edge?** Remember, we are only allowed to use at most one back edge to update `low[src]`. Since `(src, neighbor)` is already a back edge, `low[neighbor]` may introduce one more back edge!

**Why don't we update `low[src]` with `disc[neighbor]` if `(src, neighbor)` is a tree edge?** Since `neighbor` is unvisited, `disc[neighbor] > disc[src]` so `low[src]` which is initialized as `disc[src]` won't improve. Instead, `low[neighbor]` captures back edges leaving the subtree rooted at `neighbor` giving a chance for `low[src]` to improve. 

### When is `src` a cut vertex?
For any tree edge `(src, neighbor)`, if `low[neighbor] >= disc[src]`, then `src` is a cut vertex. 

**Why do we not care about back edges (src, neighbor)?** Because `src` itself will be removed if it is a cut vertex!

back edges cannot tell us about cutness. so thats why we dont care about them and only care about tree edges. not because tree edges have special properties but because theyre the only ones left by elimination.

### Issue with the root node
For *any* tree edge `(root, neighbor)`, `low[neighbor] >= disc[root]` since there can't be a back edge to some node above `root`. So, the cut condition is trivially true. 

BUT in case the root node only has one tree edge, the root node is not a cut vertex! It is only a cut vertex if it has more than one tree edge. 

Note, that I'm using the word tree edge instead of children. The root can have multiple children but only one tree edge (because the neighbor constituting the tree edge visits the rest of its children). 

One child vs Many child but all connected
Disconnected implies greater than one

### Parents do not constitute a back-edge
For any node `v`, the edge b/w `v` and its `parent` (the node which call DFS on `v`) is not a back edge. It was already classified as a tree edge by `parent`. 

## Visited already may be back edge or just a neighbor traversed by amother neighboe

### Tree edge does not mean it is isolated from the rest of the neighbors