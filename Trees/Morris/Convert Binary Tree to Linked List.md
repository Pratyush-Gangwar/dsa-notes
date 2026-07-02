## Code
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* getPre(TreeNode* root) {
        TreeNode* curr = root->left;
        while (curr->right && curr->right != root) curr = curr->right;
        return curr;
    }

    void flatten(TreeNode* root) {
        TreeNode* curr = root;

        while (curr) {
            if (!curr->left) {
                curr = curr->right;
            }

            else {
                TreeNode* left = curr->left;
                TreeNode* pre = getPre(curr);
                
                pre->right = curr->right;
                curr->left = nullptr;
                curr->right = left;

                curr = left;
            }
        }
    }
};
```

## Explanation
**Why Morris threads vs flatten doesn't:**
- **Inorder**: threads `pre->right = curr` to return to `curr` after left subtree — needs second visit to print, then restore.
- **Preorder**: same threading, needs second visit to restore the tree, not to print.
- **Flatten**: no return needed. Wire `pre->right = curr->right` permanently and move on. Tree is intentionally destroyed.

**The core rewiring:**
In Morris, we thread `pre->right = curr` to get back to `curr`. But in flatten we never need to return — after the left subtree, we just need to reach `curr->right` (the preorder successor of the left subtree's last node). So wire `pre->right = curr->right` directly.

Then, `curr->right = curr->left` — left subtree becomes the next node in the LL, since in preorder, `curr`'s immediate successor is the root of its left subtree. `curr->left = nullptr` — problem requires no left pointers in LL. Finally, advance to `left` (the saved pointer) — which is now `curr->right`, i.e. the next node in the LL

**No-left case:**
`curr` has no left child, so its `right` is already a correct LL link. Just follow it — same as inorder/preorder.
