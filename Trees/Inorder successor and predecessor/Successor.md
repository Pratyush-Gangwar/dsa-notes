## Inorder Successor in a Binary Tree
The inorder successor of a node is the node that appears immediately after it in the inorder traversal (left → root → right).

### Case 1: Right subtree exists
The successor is the **leftmost node in the right subtree**.

**Why?** After visiting the target, inorder descends into the right subtree and visits the leftmost node there first — no left child means nothing comes before it within that subtree.

### Case 2: No right subtree
The successor is the **lowest ancestor that contains the target node in its left subtree**.

**Why left and not right?** For any ancestor of the target, the target is either in its left subtree or its right subtree:
- If the target is in the **left subtree** of an ancestor, inorder visits the entire left subtree (including target) *before* visiting that ancestor. So that ancestor comes after the target — it's a candidate.
- If the target is in the **right subtree** of an ancestor, inorder visits that ancestor *before* the right subtree — so the ancestor was already visited before the target. Not a candidate.

You want the *lowest* such left-subtree ancestor because all higher ancestors also contain the target in their left subtree (transitively), so they come even later in the traversal.

**Code (with parent pointers):**
```cpp
Node* inorderSuccessor(Node* target) {
    // Case 1: right subtree exists
    if (target->right) {
        Node* cur = target->right;
        while (cur->left) cur = cur->left;
        return cur;
    }

    // Case 2: walk up until we come from a left child
    Node* successor = nullptr;
    Node* cur = target;
    while (cur->parent) {
        if (cur->parent->left == cur) {
            successor = cur->parent;
            break;
        }
        cur = cur->parent;
    }
    return successor; // nullptr if target is the last node in inorder
}
```

You climb up the ancestor chain and stop the moment you arrive at a node *from its left child* — that node is the lowest ancestor with target in its left subtree.

**Without parent pointers**, there's no shortcut: run a full inorder traversal, track the previous node, and return the current node the moment previous equals target. O(n), no way around it.

### BSTs: the shortcut
In a BST, you don't need parent pointers for Case 2. The sorted order lets you track the successor during a top-down search:

```cpp
Node* inorderSuccessorBST(Node* root, Node* target) {
    Node* successor = nullptr;
    while (root) {
        if (target->val < root->val) {
            successor = root;   // root is a left-subtree ancestor candidate
            root = root->left;
        } else {
            root = root->right; // target is in right subtree, root not a candidate
        }
    }
    return successor;
}
```

Every time you go left, the current node is a candidate (target is in its left subtree). The last such candidate recorded is the lowest one — exactly what you want. No traversal, no parent pointers: O(h).