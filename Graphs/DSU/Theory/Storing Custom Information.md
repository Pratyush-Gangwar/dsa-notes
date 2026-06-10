DSU can store custom information for each connected component in addition to the usual `parent` and `size/rank` arrays. The information is maintained at the component's **root** and updated whenever two components are merged.

```cpp
class DSU {
public:
    vector<int> parent, size;
    vector<ExtraType> extra;

    DSU(int n) {
        parent.resize(n);
        size.assign(n, 1);
        extra.resize(n);

        for (int i = 0; i < n; i++) {
            parent[i] = i;
            extra[i] = initializeExtra(i);
        }
    }

    int find(int x) {
        if (parent[x] == x) return x;
        return parent[x] = find(parent[x]);
    }

    void unite(int u, int v) {
        int pu = find(u);
        int pv = find(v);

        if (pu == pv) return;

        if (size[pu] < size[pv])
            swap(pu, pv);

        parent[pv] = pu;
        size[pu] += size[pv];

        extra[pu] = merge(extra[pu], extra[pv]);
    }
};
```

* `initializeExtra(i)` initializes the custom information for node `i`.
* `merge(extra1, extra2)` combines the information of two components.
* `extra[root]` always stores the information for the entire connected component represented by `root`.
