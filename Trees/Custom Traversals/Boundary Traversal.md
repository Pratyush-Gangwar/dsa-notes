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
    bool isLeaf(Node* node) {
        return !node->left && !node->right;
    }
  
    void leftNodes(Node* root, vector<int>& ans) {
        
        while (root) {
            if (!isLeaf(root)) ans.push_back(root->data);
            
            if (root->left) root = root->left;
            else root = root->right;
        }
        
    }
    
    void rightNodes(Node* root, vector<int>& ans) {
        vector<int> temp;
        
        while (root) {
            if (!isLeaf(root)) temp.push_back(root->data);
            
            if (root->right) root = root->right;
            else root = root->left;
        }
        
        for(int i = temp.size() - 1; i >= 0; i--) ans.push_back( temp[i] );
    }
    
    void leaves(Node* root, vector<int>& ans) {
        if (!root) return;
        if (isLeaf(root)) ans.push_back(root->data);
        
        leaves(root->left, ans);
        leaves(root->right, ans);
    }
  
    vector<int> boundaryTraversal(Node *root) {
        vector<int> ans;
        ans.push_back(root->data);
        
        if (!root->left && !root->right) return ans;
        
        leftNodes(root->left, ans);
        leaves(root, ans);
        rightNodes(root->right, ans);
        
        return ans;
        
    }
};
```

## Notes
**`isLeaf` Helper**  
 This is necessary because `leftNodes` and `rightNodes` must skip leaf nodes — without this guard, boundary nodes that happen to be leaves would get pushed twice: once by the left/right walk and again by `leaves`. 
 
**Single-Node Guard**  
The check `if (!root->left && !root->right)` in `boundaryTraversal` handles the edge case where the tree has only a root. Without it, the root gets pushed once directly, then pushed again when `leaves` visits it (since it qualifies as a leaf). The early return fires before any of the three helper calls, preventing the duplicate.