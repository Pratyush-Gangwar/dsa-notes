## Discovery Time
You know DFS — it visits nodes, going as deep as possible before backtracking. Add one thing: a **global clock** that ticks up by 1 each time you first visit a node.

```
disc[u] = the clock value when u was first visited
```

## Low
Now we define `low[u]`:
```
low[u] = the minimum disc value reachable from u's subtree
         using at most one back edge upward
```

This captures exactly what we care about — how high up the DFS tree can `u`'s subtree escape to via a back edge.

It's computed as:
```
low[u] = min(
    disc[u],
    min(low[v])   for each tree-child v of u,
    min(disc[w])  for each back edge (u, w)
)
```

The `min(low[v])` will take of back edges from the subtree rooted at `v`. But we still need to take care of back edges start at `u`. In the case there are no back edges, we use `disc[u]`.


## Cut Edges
Take a tree edge `(u, v)` where `v` is the child. Removing it cuts `v`'s subtree from `u`. The subtree reconnects iff there's a back edge from inside `v`'s subtree reaching `u` **or any ancestor of `u`**, i.e., `low[v] <= disc[u]`.

So `(u, v)` is a bridge iff `low[v] > disc[u]`.

Notice the difference from cut vertices — strict `>` instead of `>=`. If `low[v] = disc[u]`, there's a back edge from `v`'s subtree back to `u` itself. For a bridge that saves connectivity — it's a different edge than `(u, v)`, so the subtree stays connected. But for a cut vertex, reaching back to `u` doesn't help since `u` is the node being removed.