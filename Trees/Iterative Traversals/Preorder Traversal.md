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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<pair<TreeNode*, int>> stk;

        stk.push( {root, 0} );

        while (!stk.empty()) {
            auto& p = stk.top();

            TreeNode*& node = p.first;
            int& state = p.second;

            if (node == nullptr) {
                stk.pop();
                continue;
            }

            if (state == 0) {
                ans.push_back(node->val);
                stk.push( {node->left, 0} );
                state = 1;
            }

            else if (state == 1) {
                stk.push( {node->right, 0} );
                state = 2;
            }

            else if (state == 2) {
                stk.pop();
            }
        }

        return ans;
    }
};
```

## Notes
- References
- Early returns are popping.