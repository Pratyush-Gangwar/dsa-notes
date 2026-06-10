## What's the issue with standard DSU?
A standard DSU implementation usually assumes that every node `0...n-1` exists in the graph.

```cpp
vector<int> parent(n);
vector<int> size(n, 1);

for (int i = 0; i < n; i++) {
    parent[i] = i;
}
```

## When do we need Dynamic DSU?
In problems like **Most Stones Removed**, **Accounts Merge**, **Number of Islands II**, etc., the node IDs can be:
- Large (`10000`, `20000`)
- Sparse (`1`, `1000000`)
- Discovered dynamically

So people often use a **dynamic DSU**.

## Code
```cpp
unordered_map<int, int> parent;
unordered_map<int, int> size;
```

Create nodes only when needed:
```cpp
void insertIfNotExists(int x) {
    if (parent.count(x)) return;

    parent[x] = x;
    size[x] = 1;
}
```

Then before any union:
```cpp
insertIfNotExists(u);
insertIfNotExists(v);
unionBySize(u, v);
```

The find and union logic itself is unchanged. 