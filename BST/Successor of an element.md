Successor = **smallest value > x**  
This is exactly a **lower_bound(x)** problem.

## Binary Search (array)
```
ans = -1;

while (lo <= hi) {
    mid = ...

    if (arr[mid] > x) {
        ans = arr[mid];
        hi = mid - 1;   // try smaller valid
    } else {
        lo = mid + 1;
    }
}
```

## BST Conversion

Replace:

- `mid` → `root->data`
- left half → `root = root->left`
- right half → `root = root->right`

```
Node* ans = nullptr;

while (root) {

    if (root->data > x) {
        ans = root;
        root = root->left;
    } 
    else {
        root = root->right;
    }
}
	```