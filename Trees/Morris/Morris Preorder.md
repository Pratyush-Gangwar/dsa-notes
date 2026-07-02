The threading logic is identical to inorder. The predecessor being found is the inorder predecessor in both. Only the print site moves — one line up.

**Inorder:**
```cpp
if (!pre->right) {
    pre->right = curr;
    curr = curr->left;        // no print
}
else {
    ans.push_back(curr->val); // print on 2nd visit
    pre->right = nullptr;
    curr = curr->right;
}
```

**Preorder:**
```cpp
if (!pre->right) {
    ans.push_back(curr->val); // print on 1st visit
    pre->right = curr;
    curr = curr->left;
}
else {
    pre->right = nullptr;
    curr = curr->right;       // no print
}
```
