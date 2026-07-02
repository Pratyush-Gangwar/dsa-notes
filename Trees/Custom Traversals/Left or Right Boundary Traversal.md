## Left Boundary
```cpp
vector<int> leftBoundary(TreeNode* root) {
    vector<int> res;
    TreeNode* curr = root->left;
    while (curr) {
        res.push_back(curr->val);
        if (curr->left) curr = curr->left;
        else            curr = curr->right;
    }
    return res;
}
```

## Right Boundary
```cpp
vector<int> rightBoundary(TreeNode* root) {
    vector<int> res;
    TreeNode* curr = root->right;
    while (curr) {
        res.push_back(curr->val);
        if (curr->right) curr = curr->right;
        else             curr = curr->left;
    }
    return res;
}
```

Right boundary is top-down here — reverse if you need bottom-up.