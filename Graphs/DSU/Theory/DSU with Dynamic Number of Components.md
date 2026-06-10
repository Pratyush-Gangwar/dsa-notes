## Code
```cpp
class DSU {
    unordered_map<int, int> parent, rank_;
    int numComponents = 0;

public:
    // Add a new node (no-op if already exists)
    void add(int x) {
        if (parent.count(x)) return;
        parent[x] = x;
        rank_[x] = 0;
        numComponents++;
    }

    int find(int x) {
        // Auto-add on first access (optional — remove if you want strict mode)
        if (!parent.count(x)) add(x);
        parent[x] = find(parent[x]); // path compression (iterative below is safer)
        return parent[x];
    }

    bool unite(int x, int y) {
        add(x); add(y); // ensure both exist
        int px = find(x), py = find(y);
        if (px == py) return false;

        if (rank_[px] < rank_[py]) swap(px, py);
        parent[py] = px;
        if (rank_[px] == rank_[py]) rank_[px]++;

        numComponents--;
        return true;
    }

    bool connected(int x, int y) {
        if (!parent.count(x) || !parent.count(y)) return false;
        return find(x) == find(y);
    }

    int components() const { return numComponents; }
    bool exists(int x) const { return parent.count(x); }
};
```