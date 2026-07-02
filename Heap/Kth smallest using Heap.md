**Invariant:** The heap holds exactly the K smallest elements seen so far. The heap's maximum (its top) is therefore the Kth smallest.

**Optimization insight:** A new element only matters if it's smaller than the current Kth smallest (the heap's max). If it is, it should enter the bottom-K and knock out the current max. If it isn't, it should be ignored.

Always insert, then always fix — if size exceeds K, pop the max. If the new element belongs, the pop evicts the old max. If it doesn't, it becomes the new max and the pop evicts it right back out.

```cpp
int findKthSmallest(vector<int>& nums, int k) {
    priority_queue<int> maxHeap;  // max-heap by default

    for (int x : nums) {
        maxHeap.push(x);
        if ((int)maxHeap.size() > k)
            maxHeap.pop();          // evict the largest — it can't be the answer
    }

    return maxHeap.top();           // max of bottom-K = Kth smallest
}
```

**Time:** O(N log K) | **Space:** O(K)

### Why not a min-heap of K elements?
1. If you maintain a min-heap of K elements, the top is always the smallest of those K — not the Kth smallest.
2. Even if you compared against the min, the condition doesn't tell you anything useful — an element smaller than the current min obviously belongs, but so might elements larger than it.