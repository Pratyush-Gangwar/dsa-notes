```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n = nums.size();

        bool descSortedFlag = true;
        int breakIdx = n - 2;

        for(; breakIdx >= 0; breakIdx--) {
            if (nums[breakIdx] < nums[breakIdx + 1]) {
                descSortedFlag = false;
                break;
            }
        }

        if (descSortedFlag) {
            reverse( nums.begin(), nums.end() );
            return;
        }

        int newLeadIdx;
        for(int i = n - 1; i > breakIdx; i--) {
            if (nums[i] > nums[breakIdx]) { 
                newLeadIdx = i;
                break;
            }
        }

        swap( nums[newLeadIdx], nums[breakIdx] );
        reverse( nums.begin() + breakIdx + 1, nums.end() );
    }
};
```

## Why strict inequality?
We have distinct elements in a permutation.

## Does sorting break sorted order?
No, we insert the `arr[breakIdx]` element at the first position that has an element less than it. Elements at all previous indices are less than it and after it are more than it. 

## Why `reverse`?
We actually want the elements in range `[breakIdx + 1, n - 1]` (after swapping) to be arranged in ascending order. Since they're already sorted in descending, reversing them gives ascending order.

