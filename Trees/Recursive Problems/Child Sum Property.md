## Code
```cpp
/*

class Node {
public:
    int data;
    Node* left;
    Node* right;

    Node(int val) {
        data = val;
        left = nullptr;
        right = nullptr;
    }
};

*/

class Solution {
  public:
    bool helper(Node* root) {
        if (root == nullptr) return true;
        else if (!root->left && !root->right) return true;

        if (!helper(root->left)) return false;
        int leftVal = ( root->left ? root->left->data : 0 );
        
        if (!helper(root->right)) return false;
        int rightVal = ( root->right ? root->right->data : 0 );
        
        int sum = leftVal + rightVal;
        bool valid = sum == root->data;
        
        return valid;
    }
    
    bool isSumProperty(Node *root) {
        return helper(root);
    }
};
```