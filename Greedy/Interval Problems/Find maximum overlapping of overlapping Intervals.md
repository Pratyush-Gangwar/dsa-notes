## Algorithm
**Key idea:** sort isn't done on the intervals directly — it's done on the **events** derived from them. Each interval `[start, end]` produces a `(start, +1)` event and an `(end, -1)` event. Sort all `2n` events by time and sweep through, maintaining a running `depth` counter; the answer is the maximum value `depth` ever reaches.

**Why not merge intervals instead:** merging only tells you which intervals form a transitively connected cluster — it discards the timing information needed to know how many are _simultaneously_ active. E.g., `[1,10], [2,3], [4,5], [6,7]` merges into one block `[1,10]`, but the true peak concurrency is just 2 (none of the three small intervals overlap each other). The sweep line preserves instant-by-instant state; merging throws it away.

## Code
```cpp
int maxOverlap(vector<pair<int,int>>& intervals) {
    vector<pair<int,int>> events;
    for (auto& [s, e] : intervals) {
        events.push_back({s, +1});
        events.push_back({e, -1});
    }
    sort(events.begin(), events.end());

    int depth = 0, maxDepth = 0;
    for (auto& [time, delta] : events) {
        depth += delta;
        maxDepth = max(maxDepth, depth);
    }
    return maxDepth;
}
```

## O(N) alternative
**O(n) alternative (bounded integer range only):** if timestamps are bounded integers with range O(n), use a difference array indexed by time instead of sorting events — increment at `start`, decrement at `end`, then prefix-sum. This is bucket sort in disguise and only beats O(n log n) when the value range itself is linear in `n`; for large or unbounded ranges, comparison-based sorting remains the right approach.

HH:MM is sorted
2360
+1

```cpp
class Solution {
  public:
    int minPlatform(vector<int>& arr, vector<int>& dep) {
        vector<int> timeline(2360, 0);
        
        for(int i = 0; i < arr.size(); i++) {
            timeline[ arr[i] ]++;
            timeline[ dep[i] + 1 ]--;
        }
        
        int maxDepth = 0;
        int depth = 0;
        
        for(int val : timeline) {
            depth += val;
            maxDepth = max(maxDepth, depth);
        }
        
        return maxDepth;
    }
};
```