## A queer base-case
If root is $p$ or $q$, we return $root$ because the remaining node must be a descendant of $root$ and since $root$ already equals one of the nodes, we've found the LCA. This is true for both BST and BT. 

## BT (Nodes exist)
- See strivers video

## BST (Nodes exist)
- See strivers video

## Difference
- BSTs are ordered so we can exploit that property for O(log N) run-time instead of O(N) run-time. 
- Furthermore, we can convert the recursive O(N) space solution for BSTs into an iterative O(1) space solution. 