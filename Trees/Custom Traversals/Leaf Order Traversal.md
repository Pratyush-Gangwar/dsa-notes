### The invariant we want
All leaves in the left subtree should appear before all leaves in the right subtree, recursively at every node.

### Why any DFS traversal works
All three DFS traversals (pre/in/post) recurse left before right. Every leaf in the left subtree is fully processed before the right subtree is even touched, at every level of the recursion — so the invariant holds globally.

### Why the traversal label doesn't matter
The only difference between pre/in/post is where the root is processed — before, between, or after the recursive calls. But the root is only added to the answer if it is a leaf, and if it is a leaf, there are no subtrees to recurse into anyway. So the placement of the root processing never affects anything — all three traversals produce identical output for leaf collection.

### Why BFS breaks this
BFS processes nodes level by level, so a right-subtree leaf at a shallow depth will be collected before a left-subtree leaf sitting deeper. The left-before-right guarantee does not hold.