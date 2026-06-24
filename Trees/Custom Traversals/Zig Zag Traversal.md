## Code
```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (!root) return {};

        vector<vector<int>> ans;
        queue<TreeNode*> q;
        q.push(root);
        bool reverse = false;

        while (!q.empty()) {
            int size = q.size();
            vector<int> temp(size);
            
            for (int i = 0; i < size; ++i) {
                TreeNode* node = q.front();
                q.pop();

                // If reverse is true, fill from right to left; otherwise left to right
                int index = reverse ? (size - 1 - i) : i;
                temp[index] = node->val;

                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }

            reverse = !reverse;
            ans.push_back(temp);
        }

        return ans;
    }
};
```