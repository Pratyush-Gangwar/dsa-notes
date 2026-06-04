## Naive Approach
```cpp
int j = 0;
while (arr[j] < target) {
	j++;
}

return arr[j];
```

We try to replicate this logic in BSTs by performing an in-order traversal and returning once we find the largest element. This takes `O(n)` time and `O(h)` space.

```cpp
class Solution {
  public:
    int findCeil(Node* root, int x) {
        // code here
        if (root == nullptr) {
            return -1;
        }
        
        int left = findCeil(root->left, x);
        if (left != -1) return left;
        
        if (root->data >= x) return root->data;
        
        return findCeil(root->right, x);
    }
};
```

Since we're traversing the BST in-order, we know that the ceil value will be the first one to hit this condition `root->data >= x`. Therefore, if the `root` gets an answer from `left`, it must return that instead of its own value (early return).

## Binary Search Intuition
Problem:

> Smallest value >= x

Binary search intuition:
- if current value >= x:
    - possible answer
    - try finding a smaller valid one on left
- else:
    - answer must be on right

BST conversion:
```cpp
int ceil(Node* root, int x) {
    int ans = -1;

    while (root) {
        if (root->data >= x) {
            ans = root->data;
            root = root->left;
        } else {
            root = root->right;
        }
    }

    return ans;
}
```

Notice how this is literally lower_bound.