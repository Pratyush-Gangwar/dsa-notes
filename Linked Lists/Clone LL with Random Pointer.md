## Understanding the Problem
Cloning the `next` chain is easy.

The challenge is the `random` pointer:
- A node's random pointer can point anywhere in the list.
- When cloning, we need the cloned node's random pointer to point to the corresponding cloned target, not the original target.

So the problem is not creating nodes, but preserving the relationships formed by the random pointers.

## Hash Map Approach (Old → New)
Store a mapping:
```cpp
unordered_map<Node*, Node*> oldToNew;
```

### Pass 1
Create all cloned nodes and build:
```text
original node -> cloned node
```

### Pass 2
Traverse again and wire pointers:
```cpp
clone->random = oldToNew[old->random];
```

Two passes are needed because the map needs one full pass to be populated.

### Complexity
- Time: O(N)
- Space: O(N)

## One-Pass Hash Map Variant
Instead of creating every clone first, create clones on demand.

For each node:
- Ensure its clone exists.
- Ensure the clone of its `next` exists.
- Ensure the clone of its `random` exists.
- Create missing clones immediately and store them in the map.

The map still stores:
```text
original node -> cloned node
```

This allows wiring pointers during a single traversal.

### Complexity
- Time: O(N)
- Space: O(N)


## Interweaving Insight
The hash map exists because we need a way to go from an original node to its clone.
Instead of storing that relation externally, we can store it inside the linked list itself.

For every original node, insert its clone directly after it.

Transform:
```text
A -> B -> C
```

into:
```text
A (old) -> A' (clone) -> B -> B' -> C -> C'
```

Now:
```cpp
clone = old->next;
```

For random pointers:
```cpp
clone->random = old->random->next;
```

because every clone sits immediately after its original.
This removes the need for the hash map entirely.

## Optimal Three-Pass Solution

### Pass 1: Create and Interweave Clones
Convert:

```text
A -> B -> C
```

into:
```text
A -> A' -> B -> B' -> C -> C'
```

Now every original node can reach its clone through:

```cpp
old->next
```

### Pass 2: Wire Random Pointers
For each original node:

```cpp
old->next->random =
    old->random ? old->random->next : nullptr;
```

Reason:

```text
old->random
```

points to an original node, and its clone is immediately after it.

### Pass 3: Split the Lists
Separate:
```text
A -> A' -> B -> B' -> C -> C'
```

into:
```text
Original: A -> B -> C

Clone:    A' -> B' -> C'
```

This restores the original list while extracting the cloned list.


### Code
```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    void interweave(Node* head) {
        Node* curr = head;

        while (curr) {        
            Node* temp = curr->next;    

            Node* currCopy = new Node(curr->val);
            currCopy->next = temp;
            curr->next = currCopy;

            curr = temp;
        }
    }


    void wireRandoms(Node* head) {
        Node* curr = head;

        while (curr) {
            Node* currNext = curr->next;
            Node* currRandom = curr->random;

            currNext->random = (currRandom ? currRandom->next : nullptr);
            curr = currNext->next;
        }
    }

    Node* split(Node* head) {
        Node* curr = head;
        Node* newHead = head->next;

        while (curr) {
            Node* currNext = curr->next;
            if (currNext) curr->next = currNext->next;
            curr = currNext;
        }

        return newHead;;
    }

    Node* copyRandomList(Node* head) {
        if (!head) return nullptr;

        interweave(head);
        wireRandoms(head);
        return split(head);
    }
};
```
### Why Three Passes?
#### Why can’t we interweave + wire randoms in one pass?
Example:
```text
A -> B -> C
A.random = C
```

While processing `A`, you create:
```text
A -> A'
```

But this fails because `C = old_A->random` and `C->next` points to an old node.
```cpp
new_A->random = old_A->random->next;
```

#### Why can’t we wire random + split in one pass?
Interwoven:
```text
A -> A' -> B -> B'
```

At node A and A', we wired `curr->next = curr->next->next`
```text
A -> A'
B -> B'
```

Now, at node B,
```text
B.random = A
```

But now this fails because `old_A = old_B->random` and `old_A->next` is `old_B`, not `new_A`.
```cpp
new_B->random = old_B->random->next 
```

Because `A->next` is no longer `A'`, the mapping `A -> A'` is already broken, so random wiring fails.