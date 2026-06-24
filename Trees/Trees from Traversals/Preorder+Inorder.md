## The recursive structure
```
helper(preStart, inStart, inEnd):
    root = preorder[preStart]
    k = inorderIndex[root]          // split point in inorder
    leftSize = k - inStart

    root.left  = helper(preStart + 1,            inStart, k - 1)
    root.right = helper(preStart + leftSize + 1,  k + 1,  inEnd)
```

### Computing preStart for the left subtree
Preorder appends the current root, then recursively calls preorder on the left subtree. That recursive call does the same thing — appends its own root first. So the left child of the current root lands immediately at `preStart + 1`.

### Computing preStart for the right subtree
In the preorder array, starting at `preStart`: the root sits at `preStart`, followed by exactly `leftSize` nodes belonging to the left subtree (`root.left` is at `preStart + 1`), followed by the right subtree. So root.right sits at `preStart + leftSize + 1`.

### How inorder controls termination
The inorder range `[inStart, inEnd]` defines which nodes belong to the current subtree. When `inStart > inEnd` the range is empty — this subtree has no nodes, return null. Without this you'd have no way to know when to stop drilling left and switch to the right.


## The Code
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
    TreeNode* helper(int inStart, int inEnd, int preStart, unordered_map<int, int>& mp, vector<int>& preorder,       vector<int>& inorder) {
        if (inStart > inEnd) return nullptr;

        int val = preorder[preStart];
        int idx = mp[ val ];
        int leftSize = (idx - 1) - (inStart) + 1;

        TreeNode* root = new TreeNode( val );

        root->left = helper( inStart, idx - 1, preStart + 1, mp, preorder, inorder );
        root->right = helper( idx + 1, inEnd, (preStart + 1) + (leftSize + 1) - 1, mp, preorder, inorder );

        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        unordered_map<int, int> mp;
        for(int i = 0; i < inorder.size(); i++) mp[ inorder[i] ] = i;
        
        return helper(0, inorder.size() - 1, 0, mp, preorder, inorder);
    }
};
```