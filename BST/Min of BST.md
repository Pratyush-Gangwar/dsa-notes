```cpp
Node* findMin(Node* root) {
	Node* prev = nullptr;
	while (root != nullptr) {
		prev = root; 
		root = root->left;
	}
	
	return prev;
}
```

If `root == nullptr`, then `prev = nullptr` is returned. 
If `root->left == nullptr`, then `prev = root` is returned. 

It is important to be `prev` because `root` will eventually become `nullptr` so we can't return root.