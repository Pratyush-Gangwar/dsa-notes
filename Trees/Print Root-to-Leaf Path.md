## Code
```cpp
/*
Definition for Node
class Node {
  public:
    int data;
    Node* left;
    Node* right;

    Node(int val) {
        data = val;
        left = right = nullptr;
    }
};
*/

class Solution {
  public:
    void helper(Node* root, vector<int>& path, vector<vector<int>>& ans) {
        if (!root) return;
        
        path.push_back( root->val );
        
        if (!root->left && !root->right) {
            ans.push_back(path);
            path.pop_back();
            return;
        }
        
        helper(root->left, path, ans);
        helper(root->right, path, ans);
        
        path.pop_back();
    }
  
    vector<vector<int>> Paths(Node* root) {
        // code here
        vector<vector<int>> ans;
        vector<int> path;
        
        helper(root, path, ans);
        return ans;
    }
};
```

## Notes
- Only if `root` is a leaf, do we push the path in ans. 
- If `root` is not a leaf (has one/two children), we recurse since the path is not complete.
- If `root` is `nullptr` (possible because a node with one child will recursive on the `nullptr` child as well), we return. 