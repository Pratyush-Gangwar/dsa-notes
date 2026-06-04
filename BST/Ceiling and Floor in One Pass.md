## Idea
Floor and ceil are basically _perfect candidates_ for a single-pass BST walk.
They’re just two sides of the same binary-search logic, so you can track both while descending.

While walking down the BST:
- If `root->data < x` → it can be the **floor**
- If `root->data > x` → it can be the **ceil**
- If `root->data == x`, we've break early
You update both as “best seen so far” while moving like binary search.

## Code
```cpp
pair<int, int> floorCeil(Node* root, int x) {
    int floor = -1;
    int ceil = -1;

    while (root) {

        if (root->data == x) {
            floor = ceil = root->data;
            break;
        }

        if (root->data < x) {
            floor = root->data;
            root = root->right;
        }
        else {
            ceil = root->data;
            root = root->left;
        }
    }

    return {floor, ceil};
}
```

Note, we cannot use the condition `<=` or `>=` because the equality case requires special handling (`floor = ceil = root->data`).