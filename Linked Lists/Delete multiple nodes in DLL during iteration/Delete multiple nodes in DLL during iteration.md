# Delete Node in DLL Given a Reference

When given a direct reference to a node in a doubly linked list, deletion can be performed in **O(1)** time since we already know the node's position.

## Intuition

A node in a doubly linked list has pointers to both its previous and next nodes.

To delete a node:

1. Connect its previous node directly to its next node.
2. Connect its next node back to its previous node.
3. Delete the node.

Since all required pointers are already available, no traversal is needed.

## Implementation

```cpp
Node* delNode(Node* node) {
    Node* next = node->next;
    Node* prev = node->prev;

    if (next) next->prev = prev;
    if (prev) prev->next = next;

    delete node;
    return next;
}
```

## Why return `next`?

Returning the next node is not necessary for deletion itself, but it makes iteration after deletion safe and convenient.

Consider the following usage:

```cpp
Node* removeDuplicates(Node* headRef) {
    if (!headRef) return nullptr;

    Node* curr = headRef->next;

    while (curr) {
        if (curr->prev->data == curr->data) {
            bool isHead = (curr == headRef);

            curr = delNode(curr);

            if (isHead) headRef = curr;
        }
        else {
            curr = curr->next;
        }
    }

    return headRef;
}
```

After deleting `curr`, the pointer becomes invalid and cannot be dereferenced anymore. By returning the next node, we immediately obtain a valid position from which traversal can continue.

This is very similar to the iterator-erase pattern used in C++ STL containers:

```cpp
for (auto it = v.begin(); it != v.end(); ) {
    if (*it == x) {
        it = v.erase(it);  // returns next valid iterator
    }
    else {
        ++it;
    }
}
```

When an element is erased from a vector, the current iterator becomes invalid. Therefore, `erase()` returns the next valid iterator so iteration can continue safely.

Our DLL helper follows the exact same idea:

```cpp
curr = delNode(curr);
```

The deleted node can no longer be used, so the function returns the next valid node to continue traversal.

You can think of `Node*` as an iterator into a linked list and `delNode()` as the linked-list equivalent of `erase()`.

## Complexity

### Single Deletion

- **Time:** O(1)
- **Space:** O(1)

### Duplicate Removal Example

- **Time:** O(N)
- **Space:** O(1)

Each node is visited at most once.

## Edge Cases

### Deleting the Head

```cpp
prev == nullptr
```

There is no previous node, so only the next node's `prev` pointer needs updating.

### Deleting the Tail

```cpp
next == nullptr
```

There is no next node, so only the previous node's `next` pointer needs updating.

### Deleting the Only Node

```cpp
prev == nullptr && next == nullptr
```

Both pointer updates are skipped and the node is simply deleted.

The null checks naturally handle all three cases.

## Key Takeaway

Deletion in a doubly linked list is one of the few linked-list operations that can truly be performed in **O(1)** time when a direct reference to the node is available.

Returning the next node after deletion mirrors the design of STL's `erase()` and makes in-place deletion during traversal clean and safe.