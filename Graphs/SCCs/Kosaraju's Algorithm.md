## SCC
An SCC (Strongly Connected Component) is a maximal set of nodes where every node can reach every other node via directed edges.

## Condensation Graph
The **condensation** of a directed graph is what you get when you collapse each SCC into a single node. The result is always a DAG — if it had a cycle, those nodes could reach each other, meaning they should've been one SCC to begin with.

In the condensation graph, any collapsed SCC with no incoming edges from other collapsed SCCs is a source. In a DAG, there's always at least one, but there can be many.

## A node can only explore its own SCC and all SCCs below it
In the condensation DAG, edges only point downward (from sources toward sinks — that's what makes it a DAG with a notion of "above" and "below"). So when DFS starts at node i, it can follow edges forward, which means it can reach i's own SCC and any SCC reachable from it in the condensation. It cannot climb back up to SCCs that have edges pointing _into_ i's SCC.

## Finding SCCs
If you had the topological order of the condensation, you'd process supernodes from last to first — each DFS call would explore only its own SCC .
- All SCCs that come later in topological order are already visited. Any edge pointing to them is ignored.
- All SCCs that come earlier have no edges pointing back (condensation is a DAG, edges only go forward in topological order). So there are no edges to follow in that direction either.


## TopoSort
```cpp
void dfs(int u) {
    visited[u] = true;
    for (int v : adj[u])
        if (!visited[v])
            dfs(v);
    stk.push(u); // u is pushed only after all its descendants finish
}
```

Note, this is the DFS algorithm for toposort! But since our uncondensed graph is not a DAG, we can't do a toposort on it.

## What does first node visited by DFS mean?
DFS visits nodes using `if (!visited[node]) dfs(node)`. 

## The first node visited by DFS in an SCC is the last node pushed on to the stack for that SCC
Because DFS is recursive. The root's call only returns after every node reachable from it has been fully explored. Since the SCC is strongly connected, every node in the SCC is reachable from the root. So all of them are pushed onto the stack before the root is.

## If SCC C has an edge to SCC C', the first visited node in C is pushed onto the stack after all nodes in C' 

**Case 1**: Some node in C' is visited before any node in C. C' has no path back to C, all of C' is pushed onto the stack before the first node in C is even visited.

**Case 2**: Some node in C, a, is visited by DFS before any node in C'. From a, DFS can reach every node in C, and via the cross-edge into C', every node in C'. So all of C' is pushed before a. 

**Note, it is not necessary that all nodes of SCC C are pushed before all nodes of C'.** 
**Note, first node is important because its the last on the stack.**

## DFS on the Reverse Graph while popping the finish time stack
Pop nodes from the finish-time stack one by one. For each unvisited node, call `dfs(i)` on G^T. All nodes visited by that call form one SCC. Repeat until the stack is empty.

### Why on the reverse graph?
The top of the stack is a source SCC node. In G, source means no incoming cross-edges from other SCCs. Flip all edges to get G^T — those non-existent incoming edges become non-existent outgoing edges. So in G^T, DFS from this node has no cross-edges to follow outward. It can only traverse intra-SCC edges, visiting exactly the current SCC and stopping.

### Could we have avoided reversing the graph?
The bottom of the stack is a sink SCC node. In G, sinks have no outgoing cross-edges, so DFS from it stays sealed within its SCC naturally. But this would require you to pop the entire stack first. One way or another you're paying a cost. They're equivalent tradeoffs.

## Why does it work?
The top of the stack belongs to the source SCC. We completely visit it in the reversed graph. 

The next unvisited node on the stack belongs to one of the direct children of the source SCC in the condensed graph.

**Why?** It is not possible that the next unvisited node is an indirect descendant of the source SCC, because all nodes of any indirect descendant sit below all nodes of the direct children. 