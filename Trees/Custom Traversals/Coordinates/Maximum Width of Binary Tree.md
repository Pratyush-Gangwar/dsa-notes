### Why Not the Column Coordinate Approach?
Column coordinates assign `col-1` and `col+1` to children — nodes from different subtrees can land on the same column. Width here means the span between the **leftmost and rightmost node at a level**, counting all positions in between including empty ones. Column coordinates collapse overlapping positions; we need to count the gaps too.
### Complete Binary Tree Indexing
Model the tree as a complete binary tree stored in an array. If a node has index `i`, two common indexing conventions:

**0-based (root = 0):**
```
left  child → 2i + 1
right child → 2i + 2
```

**1-based (root = 1):**
```
left  child → 2i
right child → 2i + 1
```

The width of a level is then `last_index - first_index + 1` — this naturally counts empty slots in between.

BFS level by level, storing `pair<Node*, long long>` (node + its index). At each level, width = index of last node − index of first node + 1.
### The Overflow Problem
**If the tree is skewed, indices blow up exponentially — at depth `d`, the rightmost index can be `2^d`.**

Each level's rightmost index is roughly double the previous level's — because the right child formula is `2i + 2` (or `2i + 1` in 1-based). So the sequence of rightmost indices goes:

```
depth 0 → 1
depth 1 → 2
depth 2 → 4
depth 3 → 8
...
depth d → 2^d
```

For a tree of depth 63+, it overflows `long long` entirely. 

**The fix: normalize indices at the start of each level.**

At each level, subtract the index of the **first node in that level** from all indices in that level. This resets the leftmost node to index 0 at every level, keeping indices bounded by the width of that level (at most N) rather than by the absolute depth.

This is safe because width only depends on **relative** difference between indices at the same level, not their absolute values.

```cpp
// Before pushing children, normalize by subtracting level's first index
long long offset = q.front().second;  // first index at this level
// then for each node: effective index = raw_index - offset
```

---

### Code
```cpp
int widthOfBinaryTree(TreeNode* root) {
    if (!root) return 0;
    int maxWidth = 0;
    queue<pair<TreeNode*, long long>> q;
    q.push({root, 0});

    while (!q.empty()) {
        int levelSize = q.size();
        long long offset = q.front().second;  // normalize to prevent overflow
        long long first, last;

        for (int i = 0; i < levelSize; i++) {
            auto [node, idx] = q.front(); q.pop();
            long long normalizedIdx = idx - offset;

            if (i == 0) first = normalizedIdx;
            if (i == levelSize - 1) last = normalizedIdx;

            if (node->left)  q.push({node->left,  2 * normalizedIdx + 1});
            if (node->right) q.push({node->right, 2 * normalizedIdx + 2});
        }

        maxWidth = max(maxWidth, (int)(last - first + 1));
    }

    return maxWidth;
}
```

---

### Complexity

|||
|---|---|
|**Time**|O(N) — each node processed once|
|**Space**|O(N) — queue holds at most one full level|