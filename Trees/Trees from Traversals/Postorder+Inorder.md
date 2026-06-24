## Inorder + Postorder
```
helper(postEnd, inStart, inEnd):
    root = postorder[postEnd]
    k = inorderIndex[root]          // split point in inorder
    leftSize = k - inStart

    root.left  = helper(postEnd - rightSize - 1, inStart, k - 1)
    root.right = helper(postEnd - 1,              k + 1,  inEnd)
```

### Computing postEnd for the right subtree
Postorder appends the current root last, after recursively calling postorder on the right subtree. That recursive call does the same thing — appends its own root last. So the right child of the current root sits immediately at `postEnd - 1`.

### Computing postEnd for the left subtree
In the postorder array, ending at `postEnd`: the root sits at `postEnd`, preceded by exactly `rightSize = inEnd - k` nodes belonging to the right subtree (`root.right` is at `postEnd - 1`), preceded by the left subtree. So root.left sits at `postEnd - rightSize - 1`.

### How inorder controls termination
Same as before — the inorder range `[inStart, inEnd]` defines which nodes belong to the current subtree. When `inStart > inEnd` the range is empty, return null.

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
    TreeNode* helper(int inStart, int inEnd, int postEnd, unordered_map<int, int>& mp, vector<int>& postorder,       vector<int>& inorder) {
        if (inStart > inEnd) return nullptr;

        int val = postorder[postEnd];
        int idx = mp[ val ];

        int rightSize = (inEnd) - (idx + 1) + 1;

        TreeNode* root = new TreeNode( val );

        root->left = helper( inStart, idx - 1, (postEnd - 1) - (rightSize + 1) + 1, mp, postorder, inorder );
        root->right = helper( idx + 1, inEnd, postEnd - 1, mp, postorder, inorder );

        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        unordered_map<int, int> mp;
        for(int i = 0; i < inorder.size(); i++) mp[ inorder[i] ] = i;
        
        return helper(0, inorder.size() - 1, postorder.size() - 1, mp, postorder, inorder);
    }
};
```