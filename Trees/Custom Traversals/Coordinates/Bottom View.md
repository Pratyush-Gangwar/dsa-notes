## Key Idea
Same coordinate system as top view:

```
root          → (0,        0      )
left  child   → (row + 1,  col - 1)
right child   → (row + 1,  col + 1)
```

From below, the **bottommost node in each column** is visible. BFS processes nodes top-down, so the last node BFS visits in any column is the deepest one. **Always overwrite** `mp[col]` — no existence check.

Everything else is identical to top view.
### Code
```cpp
vector<int> bottomView(Node *root) {
    int maxCol = INT_MIN, minCol = INT_MAX;
    queue<pair<Node*, int>> q;
    unordered_map<int, int> mp;

    q.push({root, 0});

    while (!q.empty()) {
        auto& p = q.front();
        q.pop();
        Node* node = p.first;
        int col = p.second;

        mp[col] = node->data;
        maxCol = max(maxCol, col);
        minCol = min(minCol, col);

        if (node->left)  q.push({node->left,  col - 1});
        if (node->right) q.push({node->right, col + 1});
    }

    vector<int> ans;
    for (int col = minCol; col <= maxCol; col++)
        ans.push_back(mp[col]);

    return ans;
}
```