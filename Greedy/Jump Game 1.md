## Algorithm
Track `maxIdx` (farthest index reachable so far, considered across all positions visited). Iterate through the array with `currIdx`; at each step, first check if `currIdx > maxIdx` (meaning you're stuck — even the best of all previous jumps couldn't get you here), then update `maxIdx = max(maxIdx, currIdx + nums[currIdx])`. If the loop completes without getting stuck, check `maxIdx >= n-1`.

**Why track maxIdx instead of jumping directly to `currIdx + nums[currIdx]`:** the array allows jumping _any_ number of steps up to `nums[i]`, not exactly `nums[i]`. Committing to a single "jump to the farthest point from here" path can land you on a 0 and strand you, even if an index you jumped past had a more useful value. `maxIdx` sidesteps this entirely — it's the union of reachability across every index visited so far, not a commitment to one path, so no backtracking is ever needed.

## Code
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxIdx = 0;
        int currIdx = 0;

        while (currIdx <= maxIdx && currIdx < nums.size()) {
            maxIdx = max( maxIdx, currIdx + nums[currIdx] );
            currIdx++;
        }

        return maxIdx >= nums.size() - 1;
    }
};
```
