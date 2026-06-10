## When is it possible to make the network connected?
For a graph with `V` nodes, the minimum number of edges to make the graph connected is `V - 1` (linear graph). 

So, if `E >= V - 1`, then it is possible.

## How many edges are needed?
We need to place one edge between each connected component. 
So, we a total of `number of connected components - 1` edges. 