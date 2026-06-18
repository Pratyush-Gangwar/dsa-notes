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
    int getLen(ListNode* head) {
        int len = 0;

        while (head) {
            head = head->next;
            len++;
        }

        return len;
    }

    ListNode* rotateRight(ListNode* head, int k) {
        int len = getLen(head);
        if (len == 0) return nullptr;

        k = k % len;
        if (k == 0) return head;

        ListNode* left = head;
        ListNode* leftPrev = nullptr;
        ListNode* right = head;

        for(int i = 0; i < k - 1; i++) right = right->next;

        while (right->next) {
            right = right->next;

            leftPrev = left;
            left = left->next;
        }

        right->next = head;
        leftPrev->next = nullptr;

        return left;
    }
};

```

## Note
There is no one-pass solution since you need to know `len` before-hand to compute the true rotation `k % len`.