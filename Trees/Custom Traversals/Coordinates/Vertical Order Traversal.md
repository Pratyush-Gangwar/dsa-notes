### Key Idea
Same coordinate system:

```
root          → (0,        0      )
left  child   → (row + 1,  col - 1)
right child   → (row + 1,  col + 1)
```

Collect **all nodes per column**, top-down. Output each column's nodes sorted by `(row, val)`.

### The Sorting Problem
Top/bottom view only needed one node per column so arrival order was enough. Here we need all nodes, and the tiebreaker when two nodes share `(row, col)` is their value — **smaller value first**.

BFS gives us row grouping for free (all nodes at the same level are processed together). Within a level, nodes at the same column need to be **sorted by value** before appending. This is the only extra step over top/bottom view.

### The `temp` Map
At each BFS level, instead of pushing directly into `mp`, collect that level's nodes into a `temp` map of `col → vector<int>`. After processing the full level, sort each `temp[col]` vector and then append to `mp[col]`.

This ensures nodes at the same `(row, col)` are sorted by value before being merged into the final map — without disturbing nodes from other rows already in `mp[col]`.

### Code
```cpp
vector<vector<int>> verticalTraversal(TreeNode* root) {
    int maxCol = INT_MIN, minCol = INT_MAX;
    queue<pair<TreeNode*, int>> q;
    unordered_map<int, vector<int>> mp;

    q.push({root, 0});

    while (!q.empty()) {
        int levelSize = q.size();
        unordered_map<int, vector<int>> temp;

        while (levelSize--) {
            auto p = q.front();
            q.pop();
            TreeNode* node = p.first;
            int col = p.second;

            temp[col].push_back(node->val);
            maxCol = max(maxCol, col);
            minCol = min(minCol, col);

            if (node->left)  q.push({node->left,  col - 1});
            if (node->right) q.push({node->right, col + 1});
        }

        for (auto& [col, vec] : temp) {
            sort(vec.begin(), vec.end());
            mp[col].insert(mp[col].end(), vec.begin(), vec.end());
        }
    }

    vector<vector<int>> ans;
    for (int col = minCol; col <= maxCol; col++)
        ans.push_back(mp[col]);

    return ans;
}
```

### Why Not Just Sort the Final `mp[col]` Vectors?
You can't — that would lose row ordering. Nodes from row 2 must appear before nodes from row 5 in the same column, regardless of value. The `temp` per-level sort preserves row order while handling same-`(row, col)` ties correctly.
