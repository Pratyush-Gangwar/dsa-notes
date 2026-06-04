## How to decide whether to push the `root` into stack before or after exploring children?
Once DFS explores the entire graph, we will pop the stack. So, elements pushed into the stack *later* will appear *earlier* in the toposort. We want `root` to appear before all its children in the toposort. So, we push it *after* all the children are explored.
