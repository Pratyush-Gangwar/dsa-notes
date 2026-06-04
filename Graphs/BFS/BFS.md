## Basic BFS
```cpp
vector<int> bfsTraversal(int V, vector<vector<int>>& adjList, int start) {
    queue<int> q;
    vector<bool> visited(V, false);
    vector<int> path;

    q.push(start);
    visited[start] = true;

    while (!q.empty()) {
        int node = q.front();
        q.pop();

        path.push_back(node);

        for(int neighbor : adjList[node]) {
            if (!visited[neighbor]) {
                q.push(neighbor);
                visited[neighbor] = true;
            }
        }
    }

    return path;
}
```

### Where to put `visited[u] = true`?
We put it in the neighbor traversal code because two nodes at the same BFS level can share the same neighbor in the next BFS level. So, we don't want it being queued twice. 

### Where to perform per-node processing?
Usually, we do it after popping the node from the queue and before exploring its neighbors. 
You CAN do it inside the neighbor traversal loop. But then you'd need to handle `start` separately. And it's also not standard practice.

### Using an array for visited node only works for integer nodes
For other nodes, you must use a map. 

## BFS to discover a path from `start` to `end`
Simply check if the popped node is the `end` node. 
You can store BFS parents if you want to construct the full path. 

## Tiered/level-wise BFS
Store `levelIdx` and `levelCount`.