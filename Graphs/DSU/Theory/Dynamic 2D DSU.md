## Code
```cpp
class DSU {
    vector<int> parent, rnk;
    int C;

public:
    DSU(int rows, int cols) : C(cols), parent(rows * cols), rnk(rows * cols, 0) {
        iota(parent.begin(), parent.end(), 0);
    }

    int id(int r, int c) { return r * C + c; }

    int find(int x) {
        while (parent[x] != x) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }

    bool unite(int r1, int c1, int r2, int c2) {
        int px = find(id(r1, c1)), py = find(id(r2, c2));
        if (px == py) return false;
        if (rnk[px] < rnk[py]) swap(px, py);
        parent[py] = px;
        if (rnk[px] == rnk[py]) rnk[px]++;
        return true;
    }

    bool connected(int r1, int c1, int r2, int c2) {
        return find(id(r1, c1)) == find(id(r2, c2));
    }
};
```

## Important - Always check bounds
Consider a 3x3 grid. Say `(0, 2)` and `(1, 0)` are in the dynamic DSU.
Now explore neighbors of `(0, 2)`. Without bounds check, `(0, 3)` is a valid neighbhor (`dx = 0, dy = 1`).

Now,
```
id(0, 3) = 0 * 3 + 3 = 3
id(1, 0) = 1 * 3 + 0 = 3   ← same!
```

So without the bounds check, we connect the last cell of row 0 with the first cell of row 1, even though they're not neighbors. exists(0, 3) returns true because id(0, 3) == id(1, 0) == 3 is in the map.