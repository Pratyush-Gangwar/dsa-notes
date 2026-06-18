## Tree edges and back edges
When DFS runs, every edge in the graph gets classified. Take any edge `(u, v)`:
- If `v` is **unvisited** when we traverse it → it's a **tree edge** (we discovered `v` through `u`)
- If `v` is **already visited** → it's a **back edge**

## Tree edges form a tree 
A tree is just a (connected) graph with no cycles. 

The DFS tree is constructed node-by-node and edge-by-edge as we perform the traversal. To close a cycle, we'd need an edge between the current node and some other node already in the tree — but since both endpoints are visited, that edge is a back edge, not a tree edge. Since we only ever add tree edges to the DFS tree, no cycle can form.

## If your graph has multiple components, you get a forest
One DFS tree per connected component, and together they form a DFS forest.

## DFS Trees are not unique
It depends entirely on the order you iterate over neighbors. Different adjacency list orderings give different DFS trees, different tree edges, different back edges.

## An unvisited neighbor is always claimed by the current subtree
**Setup:** `v` is the current DFS frame and `u` is an unvisited neighbor of `v` at this point.

**Case 1:** `v` directly visits `u` — then `(v, u)` is a tree edge and `u` is a direct child of `v`.

**Case 2:** `v` visits some other neighbor first, which has some node `w` in its subtree leading to `u` . So `u` becomes a child of `w`. But since we are still inside `v`'s DFS call, `w` is in `v`'s subtree, and so `u` is also in `v`'s subtree.

**It is not possible for some node outside of `v`'s subtree to claim `u`:** DFS is recursive, so no node outside `v`'s subtree can be visited until `v`'s call returns. So whoever discovers `u` must be inside `v`'s subtree, making `v` an ancestor of `u`.


### Back Edges
If you encounter an already-visited node, it is a back edge — it always connects two nodes in an ancestor-descendant relationship.

### Current node is an ancestor
**Example**: `u` has neighbors `v` and `w`, with edge `w — u` also existing. `u` visits `v` first, `v` visits `w`. Now when `u` processes `w`, `u` is actually the **ancestor** and `w` is the descendant — yet it's still a back edge.

### Current node is a descendant
Say we have a back-edge `(u, v)`. Since we are currently visiting `u` and `v` is already visited (definition of back-edge), `v` must have been visited before `u`. But when we visited `v`, `u` was an unvisited neighbor of `v`, so `u` is in the subtree of `v` in the DFS tree — meaning `v` is an ancestor of `u`.

### Parents do not constitute a back-edge
For any node `v`, the edge b/w `v` and its `parent` (the node which call DFS on `v`) is not a back edge. It was already classified as a tree edge by `parent`. 


## The Full Graph
The DFS tree plus the back edges **is** the entire graph. There are no other edges.

