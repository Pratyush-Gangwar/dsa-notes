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
    pair<bool, int> helper(TreeNode* root) {
        if (!root) return {true, 0};

        auto leftRes = helper(root->left);
        auto rightRes = helper(root->right);
        bool currBal = ( abs( leftRes.second - rightRes.second ) <= 1 );

        bool finalBal = leftRes.first && rightRes.first && currBal;
        int currHeight = max( leftRes.second, rightRes.second ) + 1;

        return {finalBal, currHeight};

    }

    bool isBalanced(TreeNode* root) {
        return helper(root).first;
    }
};
```