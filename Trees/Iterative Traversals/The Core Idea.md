## Recursive Traversal and Zones
For a general traversal:
```cpp
dfs(node):
    // do something
    dfs(left)
    // do something
    dfs(right)
    // finish
```

Each call can be broken into **three execution zones**.
- **Zone 0 — First time at node**
	- We have just entered the function
	- No recursive calls have happened yet

- **Zone 1 — After left subtree**
	- Left recursive call is fully done
	- Right subtree is still pending

- **Zone 2 — After right subtree**
	- Both subtrees are done
	- Function is ready to return

## Problem with naive stack simulation
If we only store:

```cpp
(node)
```

Then, when we pop a node, we cannot know:
- Is this the first visit?
- Are we returning from left?
- Are we returning from right?

The node alone does not encode this control flow state.

## Solution: Explicit state tracking
We store a pair:

```cpp
(node, state)
```

Where:
- `state = 0` → Zone 0 (first time visiting node)
- `state = 1` → Zone 1 (returned from left subtree)
- `state = 2` → Zone 2 (returned from right subtree)

## How state simulates recursion flow

### Initial step

```cpp
push(root, 0)
```

### Case 1: state = 0 (Enter node)
We are entering the node for the first time.
We execute Zone 0 work.
We set our state to `1`.
And then proceed to the left subtree.

```cpp
state = 1
push(node->left, 0)  // go left first
```

---

### Case 2: state = 1 (After left subtree)
Left recursion is complete. 
We execute Zone 1 work.
We set our state to `2`.
Now we proceed to the right subtree:

```cpp
state = 2
push(node->right, 0)
```

### Case 3: state = 2 (After right subtree)
Both children are done.
We execute Zone 2 work.
We pop the node. 
