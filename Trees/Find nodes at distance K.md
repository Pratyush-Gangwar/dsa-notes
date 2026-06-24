## Code
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, unordered_map<TreeNode*, TreeNode*>& parent) {
        if (!root) return;

        if (root->left) parent[root->left] = root;
        if (root->right) parent[root->right] = root;

        dfs(root->left, parent);
        dfs(root->right, parent);
    }

    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        vector<int> ans;

        unordered_map<TreeNode*, TreeNode*> parent;
        dfs(root, parent);

        queue<TreeNode*> q;
        unordered_set<TreeNode*> visited;

        q.push( target );
        visited.insert( target );

        int levelCount = 0;

        while (!q.empty()) {
            int levelSize = q.size();

            while (levelSize) {
                TreeNode* node = q.front();
                q.pop();
                levelSize--;
                
                if (levelCount == k) {
                    ans.push_back(node->val);
                    continue;
                }

                if (node->left && !visited.count(node->left)) {
                    visited.insert(node->left);
                    q.push( node->left );
                }

                if (node->right && !visited.count(node->right)) {
                    visited.insert(node->right);
                    q.push( node->right );
                }

                if (parent.count(node) && !visited.count( parent[node] )) {
                    visited.insert( parent[node] );
                    q.push( parent[node] );
                }

            }

            if (levelCount == k) break;
            levelCount++;
        }

        return ans;
    }
};
```