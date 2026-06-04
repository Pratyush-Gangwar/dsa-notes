## If `src` is the root of the TopoSort
Here is the concise, step-by-step structural logic using the sequence $[u_1, u_2, u_3, \dots, u_n]$, where $dist[u_1] = 0$ and all others start at $\infty$.

**Fundamental Property:** For any directed edge from ui​→uj​, the source node ui​ must appear before the destination node uj​ in the list (i<j).

* **At $u_1$ (The Root):** It has zero incoming edges, so $dist[u_1] = 0$ is instantly **final**. It relaxes its outgoing edges, updating the distances to its neighbors (like $u_2$ and $u_3$).
* **At $u_2$:** The only possible incoming edges must come from nodes to its left. The only node to its left is $u_1$. Because the shortest path to $u_1$ is already **final** and it has already run its updates, $dist[u_2]$ is now **final**. $u_2$ then relaxes its outgoing edges to nodes further down the line.
* **At $u_3$:** Incoming edges can only come from $u_1$ and $u_2$. Because the shortest paths to these nodes on the left are already **final** and they have already run their updates, every possible path to $u_3$ has been evaluated. Thus, $dist[u_3]$ is now **final**, and it propagates its value forward.
* **At $u_i$:** By the time the pointer reaches any arbitrary node $u_i$, the shortest paths to all nodes on its left are already **final** and they have already run their updates. Since no node to the right can ever point backward, there are no remaining paths left to discover. 

Every distance is locked in permanently on a single, linear pass.

## If `src` is not the root of the TopoSort
If the source node $S$ starts later in the sequence—say at $u_k$—the exact same structural logic holds for all reachable nodes. We initialize $dist[S] = 0$ and all other nodes to $\infty$.

* **For any node to the left of $S$ (indices $< k$):** These nodes can never be reached from $S$ because edges only flow forward in a toposort. Their distances correctly remain at $\infty$ and are instantly **final**.

* **At $S$ (The Source $u_k$):** $dist[S] = 0$ is **final** by convention. $S$ runs its updates and relaxes all its outgoing edges forward.

* **At the next node, $u_{k+1}$:** The incoming edges must come from nodes to its left. Although there can be paths from the $\infty$-distance nodes to $u_{k+1}$, they are not reachable from $S$ (no backward edges; only forward edges in toposort) and are hence not valid. So, only $S$ is valid. Because the shortest path to $S$ is already **final** and it has already run its updates, $dist[u_{k+1}]$ is now **final**. It then relaxes its outgoing edges further down the line.

* **At any subsequent node $u_i$ (indices $> k$):** By the time the pointer reaches $u_i$, the shortest paths to all nodes on its left are already **final** and they have already run their updates. 

Since no node to the right can ever point backward, every node downstream of $S$ receives its optimal distance on a single, linear pass.