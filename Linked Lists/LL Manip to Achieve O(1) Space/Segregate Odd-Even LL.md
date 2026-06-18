## Code
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }

        ListNode* oddHead = head;
        ListNode* oddCurr = oddHead;

        ListNode* evenHead = head->next;
        ListNode* evenCurr = evenHead;

        ListNode* curr = evenHead->next;

        oddHead->next = nullptr;
        evenHead->next = nullptr;

        int i = 3;

        while (curr != nullptr) {
            ListNode* temp = curr->next;
            curr->next = nullptr;

            if (i % 2 == 0) {
                evenCurr->next = curr;
                evenCurr = evenCurr->next;
            }

            else {
                oddCurr->next = curr;
                oddCurr = oddCurr->next;
            }

            curr = temp;
            i++;
        }

        oddCurr->next = evenHead;
        return oddHead;
    }
};
```

## Why Do We Break Links?

As nodes are moved from the original list into the odd and even lists, we explicitly sever their old connections:

```cpp
oddHead->next = nullptr;
evenHead->next = nullptr;
```

and later:

```cpp
ListNode* temp = curr->next;
curr->next = nullptr;
```

This ensures that every node being appended is treated as a standalone node before being attached to its new list.

Without breaking these links, nodes would still point to parts of the original list, causing the odd and even sublists to accidentally remain connected. This can lead to:

- Incorrect list structure
- Duplicate nodes appearing in both lists
- Cycles or unexpected traversals

The pattern is similar to "cutting" a node out of one chain before "splicing" it into another.

A useful invariant is:

> Every node in the odd and even lists should have a valid `next` pointer only because we explicitly assigned it there, never because it retained an old link from the original list.