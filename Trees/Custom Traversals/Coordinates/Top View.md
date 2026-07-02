## Key Idea
Assign every node a 2D coordinate `(row, col)`:

```
root          → (0,        0      )
left  child   → (row + 1,  col - 1)
right child   → (row + 1,  col + 1)
```

From above, only the **topmost node in each column** is visible — every node below it in the same column is blocked. So the answer is: **first node per column, in column order.**
### Why BFS
BFS processes nodes top-down, level by level. This guarantees the first time any column is encountered, it's being populated by the node with the smallest row in that column.
### Data Structures
**Queue** stores `pair<Node*, int>` — the node and its `col` coordinate. `row` is not stored since BFS arrival order already encodes top-down priority.

**`unordered_map<int, Node*>`** maps `col → node`. Insert only if the column is unseen; BFS guarantees the first insertion is always the topmost node in that column.

**`minCol` / `maxCol`** tracked during insertion. Since columns shift by exactly `±1` at each edge, every integer between `minCol` and `maxCol` is occupied — no holes. Iterating `[minCol, maxCol]` gives the answer in sorted column order without needing an ordered map.

> `unordered_map` + min/max tracking gives O(1) insertion. `map` gives O(log N) insertion and buys you sorted order — but you don't need it if you track min/max yourself.

### Code
```cpp
vector<int> topView(Node *root) {
    int maxCol = INT_MIN, minCol = INT_MAX;
    queue<pair<Node*, int>> q;
    unordered_map<int, int> mp;

    q.push({root, 0});

    while (!q.empty()) {
        auto [node, col] = q.front();
        q.pop();

        if (!mp.count(col)) {
            mp[col] = node->data;
            maxCol = max(maxCol, col);
            minCol = min(minCol, col);
        }

        if (node->left)  q.push({node->left,  col - 1});
        if (node->right) q.push({node->right, col + 1});
    }

    vector<int> ans;
    for (int col = minCol; col <= maxCol; col++)
        ans.push_back(mp[col]);

    return ans;
}
```
