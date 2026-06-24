## The Core Tension

The problem asks for the maximum sum path in a binary tree, where a path is any sequence of nodes with no repeated nodes — it can start and end anywhere. The tricky part: **this problem has no clean recursive formulation directly**.

To solve it, we split the work across two things: a recursive helper with a clean recurrence, and a global maximum updated as a side effect.

## The Helper: Max Root-to-Leaf Sum

Define `helper(node)` = maximum sum path starting at `node` and going **downward** — either into the left subtree or the right subtree, but not both.

This is the key constraint that makes a clean recurrence possible. At any node, you either go left or go right — never both. So:

```
helper(node) = node.val + max(helper(left), helper(right), 0)
```

The `0` handles the case where both children would drag the sum down — in that case, we just don't extend downward at all (the path ends at `node`).

**Why include `node.val` unconditionally?**  
Because the path must be contiguous — no holes. If we're computing the best path rooted at `node`, `node` itself must be in it. A negative `node.val` is still acceptable here because some ancestor called us specifically to include this node as part of its downward extension.

**Why clamp children at 0 but not root?**  
If `helper(left) < 0`, then `node.val + helper(left) < node.val`, so we're strictly better off not going left. Children are optional extensions. But `node` itself is mandatory — whoever called us is counting on it.

## Why This Doesn't Solve the Original Problem
`helper` only returns paths that go **one way** from a node. But the maximum sum path in the tree might be a **bridge** — entering from the left subtree, passing through a node, and continuing into the right subtree. This shape can never be returned up the call stack, because once you return it to a parent, the parent would be trying to extend a path that already has two directions — which breaks the no-branching constraint.

So `helper` can find the best downward path from any node, but it can never *return* a bridge.

## Bringing in the Global Maximum
At every node, the candidate paths through it are exactly:

| Shape | Value |
|---|---|
| Go left only | `node.val + max(helper(left), 0)` |
| Go right only | `node.val + max(helper(right), 0)` |
| Bridge | `node.val + max(helper(left), 0) + max(helper(right), 0)` |

We compute all three and update a global `max_sum` before returning. `helper` itself still returns only the best one-directional path (for the parent's use), but the bridge candidate gets considered via the side effect.

```python
def helper(node):
    if not node:
        return 0
    
    left  = max(helper(node.left), 0)
    right = max(helper(node.right), 0)
    
    # Best path through this node (bridge or single-arm)
    max_sum[0] = max(max_sum[0], node.val + left + right)
    
    # Return only one direction to parent
    return node.val + max(left, right)
```

The global update runs at every node, so every possible bridge gets evaluated exactly once — when we're sitting at the node that forms its peak.