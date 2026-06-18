## The Linked List
```
0 → nums[0] → nums[nums[0]] → ... → d → ... → d → ...
                                    ↑           │
                                    └───────────┘
```

## Why include 0 separately?
We need a head for the linked list.

Values are in `[1..n]`, so **no value in the array equals `0`**. That means `0` is never the destination of any jump — nothing points to it.

If you start anywhere in `[1..n]`, you might start _inside_ the cycle already. Then Floyd's slow/fast would still detect a meeting point, but we won't be able to find a head for the LL because circular linked lists have no head!

`0` is guaranteed to be outside the cycle. That's the only reason it's chosen.

## The first time an already visited number is visited
Yes — the chain `0 → nums[0] → nums[nums[0]] → ...` explores new indices until it visits an already visited number again. At that point, the chain closes into a loop and never leaves

Everything else in the array — other non-duplicates, the more "copies" of the duplicate — is simply never visited. They're unreachable from `0` and irrelevant.

## Code
```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0;
        int fast = 0;

        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return fast;
    }
};
```
