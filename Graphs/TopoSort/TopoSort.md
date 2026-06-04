## DFS vs Khan's Algorithm
- Use DFS if you know it's a DAG. It's shorter than Khan's.
- Use Khan if don't know that it's a DAG (that is, you want to find the toposort if it's a DAG or report that there's a cycle if it's not a DAG). It's easier to find cycles in Khan's as compared to DFS.