## The Idea

You can treat *some* search problems on BSTs like binary search on sorted arrays. In most cases, iterative approaches work perfectly for these, reducing space complexity to **O(1)**.

The key shift is to stop thinking of a BST as a *tree problem* first, and instead think:

> “At every node, can I eliminate half the search space?”

If yes, the solution often falls out almost mechanically.

---

## Invariant

A BST gives you this invariant:

```
left subtree  <  root  <  right subtree
```

This is the exact same structure binary search relies on in arrays:

```
left side  <  mid  <  right side
```

So whenever a problem asks for:

- closest value  
- ceil / floor  
- predecessor / successor  
- insert / search / delete  
- lower bound / upper bound  
- kth smallest / largest  
- range queries  

…you should immediately ask:

> “What would binary search do on a sorted array?”

Then translate that logic directly onto the BST.

---

## Step-by-Step Conversion Pattern

### Step 1: Imagine the inorder traversal

Example BST:

```
        8
      /   \
     4     12
    / \    / \
   2   6  10 14
```

The inorder traversal of a BST is sorted:

```
[2, 4, 6, 8, 10, 12, 14]
```

Now you can mentally treat the BST like this sorted array.

---

### Step 2: Solve it as a binary search problem

Example: *Find the ceil of x*

Binary search intuition:

```
If mid >= x:
    this could be the answer
    but there might be a smaller valid one on the left

Else:
    go right
```

That logic is the real solution.

---

### Step 3: Replace “mid” with “current node”

Array binary search:

```
mid = arr[m]
```

BST version:

```
root->data
```

Now convert movement:

| Binary Search      | BST                |
|-------------------|--------------------|
| search left half  | root = root->left  |
| search right half | root = root->right |

That’s the entire translation.

---

## Iterative Pattern (Core Idea)

Most BST search problems follow this structure:

### 1. Traverse iteratively

At every step, you only move in one direction:

- left  
- or right  

So recursion is often unnecessary.

---

### 2. Keep an external answer variable

Why?

Because while traversing, the current node may be a **valid candidate**, but not necessarily the BEST one.

So you:

- store it
- continue searching for a better answer

This is exactly the same idea as `lower_bound` / `upper_bound` in binary search.

---

## Generic Skeleton

```
Node* ans = nullptr;

while (root) {

    if (current node is a valid candidate) {
        ans = root;

        // try to improve answer
        move toward a better candidate side
    }
    else {
        move in the opposite direction
    }
}
```
```