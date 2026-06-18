## Why does it work?
Assume a loop doesn't exist. Then, the fast pointer will become `nullptr`.

Now assume a loop does exist. Then, eventually both the slow and fast pointers will enter the loop. Then, they must meet at some point since two objects on a circular path with different speeds always meet. 

## Code
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;

        while (true) {
            if (fast == nullptr || fast->next == nullptr) return false;
            
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast) return true;
        }

        return false;
    }
};
```

We need to check `fast == nullptr` because we access `fast->next`. We need to need `fast->next == nullptr` because we check `(fast->next)->next`.

## Would it work even for circular linked lists?
![[Pasted image 20260614222148.png]]

Yeah. Two pointers at different speeds in a loop always meet. 

In linked lists which have a chain before the loop, the only purpose of the chain is to bring both slow and fast pointers into the loop. That's it. It's not necessary for correctness.