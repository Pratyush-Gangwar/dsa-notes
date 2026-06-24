## What inorder traversal actually needs
After visiting a node, you need its inorder successor. The recursive stack exists purely to answer that question:
- **Right subtree exists:** successor is the leftmost node of the right subtree. Recurse right.
- **No right subtree:** you need to go back up. If you arrived as a left child, your parent is the successor. If you arrived as a right child, keep popping until you arrive as a left child — that ancestor is your successor.

The key observation: `curr`'s inorder predecessor is the **rightmost node of `curr`'s left subtree**. That node's right pointer is always `null` by definition. We borrow it to store a thread back to `curr`, so when traversal reaches the predecessor, instead of needing to pop a stack, it follows the thread and lands directly at its successor.

## The algorithm
For each `curr`:
- **No left subtree*: Just print `curr` and move to the right subtree. 
- **Left subtree exists** — find the rightmost node of it (the inorder predecessor). Two cases:
	- **No thread yet (first visit):** Set `predecessor->right = curr`. Move `curr` left. The return path is now wired.
	- **Thread exists (second visit):** You arrived here by following a thread. Print `curr`. Restore `predecessor->right = null`. Move `curr` right.

### No left subtree
When `curr->left == NULL`, the inorder predecessor of `curr` is the lowest ancestor which has `curr` in its right subtree. We don't set a thread from it to `curr` for two reasons:
1. **It has already been visited.** There is no need to return to `curr` from it — contrast this with the left-subtree case, where the predecessor hasn't been visited yet, and once it is visited, we need a thread back up to `curr` so the traversal can continue from there.
2. **Its right pointer is already non-null** — either a real right child or an existing thread. So storing a thread there would corrupt the tree irreversibly.

### Right subtree will always exist except once
`curr` is somebody's inorder predecessor, so `curr->right` is already set — either a real right child or a thread. It can exit on its own. No external thread from `curr` is needed.

The only exception is if `curr` is the inorder-last node — it's nobody's predecessor, so its right is null, and the traversal terminates there.

