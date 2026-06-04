## Idea
You can find **both predecessor and successor in one pass** because you’re doing a single root-to-leaf walk, just updating two best candidates as you go.

While traversing:
- If `root->data < x` → it’s a **candidate for predecessor**
- If `root->data > x` → it’s a **candidate for successor**
- If `root->data == x`, we break early by finding predecessor as maximum in left subtree and successor as minimum in right subtree.

You still move like binary search:
- go right if value < x
- go left if value > x

So you refine both answers in one traversal.

## Code
```cpp
pair<Node*, Node*> findPreSuc(Node* root, int x) {
    Node* pred = nullptr;
    Node* succ = nullptr;

    while (root) {

        if (root->data < x) {
            pred = root;
            root = root->right;
        }
        else if (root->data > x) {
            succ = root;
            root = root->left;
        }
        else {
            // exact match: check subtrees once
            Node* t = root->left;
            while (t) {
                pred = t;
                t = t->right;
            }

            t = root->right;
            while (t) {
                succ = t;
                t = t->left;
            }
            break;
        }
    }

    return {pred, succ};
}
```

Note, we cannot have just `>` and `<` and not `==` case because in case `x` exists in the tree, then our loop will break without having found the correct answer.

Note, when `root->data == x`, we do not calculate the min/max of the subtree including `root`. Rather, we look at `root->left` and `root->right` since successor/predecessor are defined with strict inequality.