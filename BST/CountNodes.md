### Related terms
- **Full binary tree**: every node has 0 or 2 children (no “single child” nodes)
- **Perfect binary tree**: every level is completely filled (including last level)
- **Complete binary tree**: last level is left-filled, but may not be full

## The Idea
If the left height and right height of a complete binary tree are equal, then it is a perfect binary tree. In that case, the number of nodes is `2^h - 1` where `h` is the left/right height including the root.

Note that this `leftHeight == rightHeight` condition being sufficient to be a perfect binary tree only works because complete binary trees are filled left-to-right in the last level. There are no "gaps" in the last level. In a general binary, there may be "gaps" and this condition would not be sufficient.

Since any subtree of a complete binary tree is also a complete binary tree, we can use the recurrence relation `countNodes(root) = 1 + countNodes(root->left) + countNodes(root->right)` in case the above short-circuit doesn't work.

## Wrong Code
```cpp
class Solution {
public:
    int getHeight(TreeNode* root) {
        if (root == nullptr) return 0;
        return 1 + max(getHeight(root->left), getHeight(root->right));
    }   
    
    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;

        int leftHeight = getHeight(root->left);
        int rightHeight = getHeight(root->right);

        if (leftHeight == rightHeight) return pow(2, leftHeight) - 1;

        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

The classical height of the left and right subtrees for any node in a complete binary tree can be the same even if it is not a perfect binary tree. Height is too coarse - it doesn’t capture where missing nodes are. Look at this example.

![[Pasted image 20260527075501.png]]

What we want instead is the number of nodes in the left and right *arms*.

## Correct Code
```cpp
class Solution {
public:
    int getLeftHeight(TreeNode* root) {      
        int leftHeight = 0;
        while (root != nullptr) {
            leftHeight++;
            root = root->left;
        }

        return leftHeight;
    }   

    int getRightHeight(TreeNode* root) {      
        int rightHeight = 0;
        while (root != nullptr) {
            rightHeight++;
            root = root->right;
        }

        return rightHeight;
    }   
    
    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;

        int leftHeight = getLeftHeight(root);
        int rightHeight = getRightHeight(root);

        if (leftHeight == rightHeight) return pow(2, leftHeight) - 1;

        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

Do note that the early return in `if (leftHeight == rightHeight)` is important for ensuring less than `O(n)` complexity by preventing `1 + countNodes(root->left) + countNodes(root->right)` from running.

## Complexity
In a complete but imperfect binary tree, it is not possible for BOTH the left and right subtrees to be complete but imperfect. It would introduce "gaps". 

The cases are:
- left is perfect, right is perfect
- left is perfect, right is not perfect

Left subtree cannot be imperfect while right subtree is perfect either.

So, realistically, although we recursively call on both subtrees, we only ever traverse down the right subtree. So, our recursion depth is `O(h) = O(log N)`.

Total complexity is `O(log^2 N)`