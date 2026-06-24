## What each traversal gives you
- **Inorder** — given the root, everything to its left in the sequence is in the left subtree, everything to its right is in the right subtree.
- **Preorder / Postorder** — gives you the root (first / last element).

Neither preorder nor postorder alone can give you the left/right split, and inorder alone cannot tell you which element is the root. They perfectly complement each other.

The root's position in inorder equals the size of the left subtree, which can be anything from 0 (fully right-skewed) to n-1 (fully left-skewed). 

The "root is the middle element" intuition only holds for perfectly balanced trees. 
## Why distinct elements are required
At each recursive call you need to find the root's position in the inorder array. A naive linear scan costs O(n) per call, giving O(n²) overall. Instead you precompute a `value → index` hashmap once upfront, making each lookup O(1) and the overall algorithm O(n).

This hashmap only works correctly if every value maps to exactly one index — i.e. all elements are distinct. If duplicates existed, multiple positions would match the root's value and the split would be ambiguous. 
