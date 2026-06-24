## A single traversal isn't enough
Any single traversal output is ambiguous:

```
    1                1
   /                  \
  2                    2
```

Both give inorder, preorder, postorder, level order `[1, 2]` — indistinguishable. The traversal tells you *which* nodes exist but not *where* they sit structurally.

---

## Adding nulls makes it unique
If you include null markers for missing children, structure is fully encoded:

```
Tree 1 preorder: "1 2 # # #"
Tree 2 preorder: "1 # 2 # #"
```

Now unambiguous. This works for any traversal — preorder, postorder, inorder, or level order — as long as nulls are included.

## Naive level order with nulls → exponential space
The obvious approach: at every level, emit all children including nulls, even children of null nodes.

```
    1
     \
      2
       \
        3

Level 0: 1
Level 1: # 2
Level 2: # # # 3
Level 3: # # # # # # # #
```

Tokens double every level → $O(2^h)$ space. For a skewed tree, $h = n$, so total space is $O(2^n)$.

## Level Order Serialization
### The idea
Do a standard BFS, enqueuing both children (null or not) for every non-null node you dequeue. When you dequeue a node, emit its value — or `#` if it's null, in which case don't enqueue anything.

This gives you the tree level by level, with nulls marking missing positions.
```
        1
       / \
      2   3
     / \   \
    4   5   6

Output: "1 2 3 4 5 # 6 # # # # # #"
```

Breaking it into levels:
```
Level 0: 1
Level 1: 2 3
Level 2: 4 5 # 6
Level 3: # # # # # #
```

### Serialization
```cpp
string serialize(TreeNode* root) {
    if (!root) return "";
    string result = "";
    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        TreeNode* node = q.front(); q.pop();

        if (node == nullptr) {
            result += "# ";
        } else {
            result += to_string(node->val) + " ";
            q.push(node->left);
            q.push(node->right);
        }
    }
    return result;
}
```

### Deserialization
Let `s_i` = number of non-null nodes at level `i`. At the start of each iteration:
- Queue holds the nodes of the current level (null and non-null)
- String pointer sits at the start of the next level's tokens

Each dequeue either emits `#` and contributes nothing, or emits a value and enqueues two children. After the entire current level is processed, the queue holds exactly the next level's nodes and the pointer is at the level after that's tokens. Invariant preserved.

```cpp
TreeNode* deserialize(string data) {
    if (data.empty()) return nullptr;
    istringstream ss(data);
    string token;

    ss >> token;
    TreeNode* root = new TreeNode(stoi(token));
    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        TreeNode* node = q.front(); q.pop();

        ss >> token;
        if (token != "#") {
            node->left = new TreeNode(stoi(token));
            q.push(node->left);
        }

        ss >> token;
        if (token != "#") {
            node->right = new TreeNode(stoi(token));
            q.push(node->right);
        }
    }
    return root;
}
```

### Size of string
The serialized string has at most `3n` tokens — each of the `n` non-null nodes contributes at most 3 tokens: itself and two null children. So string length is O(n).

## Preorder alternative
If you don't need level order format, preorder with nulls is cleaner — recursion handles everything, no queue needed:

```cpp
void serialize(TreeNode* root, ostringstream& out) {
    if (!root) { out << "# "; return; }
    out << root->val << " ";
    serialize(root->left, out);
    serialize(root->right, out);
}

TreeNode* deserialize(istringstream& in) {
    string val; in >> val;
    if (val == "#") return nullptr;
    TreeNode* root = new TreeNode(stoi(val));
    root->left = deserialize(in);
    root->right = deserialize(in);
    return root;
}
```

The call stack *is* the traversal — no invariant to maintain, no queue to track. Lead with this in interviews unless level order format is required.

Note, you can use pre/inorder too. 

## Code notes
**`if (!root) return ""`** — edge case for an empty tree. Without it you'd push a null root into the queue and immediately hit undefined behavior on `node->val`.

**`if (data.empty()) return nullptr`** — symmetric edge case. An empty string means the original tree was empty, so return null. Without it `ss >> val` would fail to read anything and you'd call `stoi("")` which throws.

**`string val` not `int`** — you can't `>>` directly into an int when the token might be `#`. Read as string first, check for `#`, then `stoi` only if it's a real value.

**`ostringstream`** — lets you build a string with `<<` the same way you'd print to `cout`. Retrieve the final string with `ss.str()`. The alternative is `result += to_string(node->val) + " "` which is equally fine in an interview.

**`istringstream`** — lets you read tokens one by one with `>>` the same way you'd read from `cin`. Automatically splits on whitespace, advances the internal pointer for you. The alternative is manually tracking an index into the string and extracting substrings yourself — more code for no benefit.