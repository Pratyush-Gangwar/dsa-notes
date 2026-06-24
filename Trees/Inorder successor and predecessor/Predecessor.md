## Inorder Predecessor in a Binary Tree
The inorder predecessor of a node is the node that appears immediately before it in the inorder traversal (left → root → right).

It is the exact mirror of the successor problem.

### Case 1: Left subtree exists
The predecessor is the **rightmost node in the left subtree**.

**Why?** Inorder visits the left subtree entirely before the target. The last node visited in a subtree is always its rightmost node — it has no right child, so nothing comes after it within that subtree.

### Case 2: No left subtree
The predecessor is the **lowest ancestor that contains the target node in its right subtree**.

**Why right and not left?** For any ancestor of the target:

- If the target is in the **right subtree** of an ancestor, inorder visits that ancestor *before* descending into the right subtree — so that ancestor comes before the target. It's a candidate.
- If the target is in the **left subtree** of an ancestor, inorder visits the entire left subtree (including target) *before* that ancestor — so the ancestor comes after the target. Not a candidate.

You want the *lowest* such right-subtree ancestor because all higher ancestors also contain the target in their right subtree transitively, so they come even earlier in the traversal.

**Code (with parent pointers):**

```cpp
Node* inorderPredecessor(Node* target) {
    // Case 1: left subtree exists
    if (target->left) {
        Node* cur = target->left;
        while (cur->right) cur = cur->right;
        return cur;
    }

    // Case 2: walk up until we come from a right child
    Node* predecessor = nullptr;
    Node* cur = target;
    while (cur->parent) {
        if (cur->parent->right == cur) {
            predecessor = cur->parent;
            break;
        }
        cur = cur->parent;
    }
    return predecessor; // nullptr if target is the first node in inorder
}
```

You climb up the ancestor chain and stop the moment you arrive at a node *from its right child* — that node is the lowest ancestor with target in its right subtree.

**Without parent pointers**, same situation as successor: full inorder traversal, track the previous node, and return previous the moment current equals target. O(n).


### BSTs: the shortcut
Mirrored logic — every time you go right, the current node is a candidate:

```cpp
Node* inorderPredecessorBST(Node* root, Node* target) {
    Node* predecessor = nullptr;
    while (root) {
        if (target->val > root->val) {
            predecessor = root;  // root is a right-subtree ancestor candidate
            root = root->right;
        } else {
            root = root->left;   // target is in left subtree, root not a candidate
        }
    }
    return predecessor;
}
```

Every time you go right, the current node is a candidate (target is in its right subtree). The last such candidate is the lowest one. O(h), no traversal needed.