## The Problem
Given an array `nums` and integer `k`, return the Kth largest element in sorted order (not Kth distinct).

---

## Approach 1 — Full Array in Min-Heap (Naive)
Push everything into a min-heap of size N, then pop K-1 times. The top is your answer.

```cpp
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap(nums.begin(), nums.end());
    for (int i = 0; i < k - 1; i++)
        minHeap.pop();
    return minHeap.top();
}
```

**Time:** O(N + K log N) — O(N) to heapify, O(K log N) for K pops  
**Space:** O(N)

Works, but you're storing the entire array in the heap. The K pops are also wasteful if K is small.

---

## Approach 2 — Bounded Min-Heap of Size K (Optimal)
**Invariant:** The heap holds exactly the K largest elements seen so far. The heap's minimum (its top) is therefore the Kth largest.

**Optimization insight:** A new element only matters if it's larger than the current Kth largest (the heap's min). If it is, it should enter the top-K and knock out the current min. If it isn't, it should be ignored. 

Instead of checking `val > heap.top()` before inserting, the cleaner way to express this is: **always insert, then always fix** — if size exceeds K, pop the min. If the new element belongs, the pop evicts the old min. If it doesn't, it becomes the new min and the pop evicts it right back out.

```cpp
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;

    for (int x : nums) {
        minHeap.push(x);
        if ((int)minHeap.size() > k)
            minHeap.pop();          // evict the weakest — it can't be the answer
    }

    return minHeap.top();           // min of top-K = Kth largest
}
```

**Time:** O(N log K) — each of the N elements does one push + at most one pop, both O(log K)  
**Space:** O(K)

This is also directly the solution to **LeetCode 703 (Kth Largest in a Stream)** — the `add()` method is just this loop body, one element at a time.

### Why not consider a max heap of k elements?
1. If you maintain a max-heap of K elements, the top is always the largest of those K — not the Kth largest.
2. Even if you compared against the max, the condition doesn't tell you anything useful — an element larger than the current max obviously belongs, but so might elements smaller than it.

---

## Complexity Comparison

|Approach|Time|Space|
|---|---|---|
|Full heap, pop K times|O(N + K log N)|O(N)|
|Bounded heap of size K|O(N log K)|O(K)|

When K << N, the bounded heap is significantly faster in both dimensions.