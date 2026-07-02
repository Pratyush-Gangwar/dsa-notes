## The Core Problem Family

Four variants, two axes:

| | **Array** | **Stream** |
|---|---|---|
| **Kth Largest** | QuickSelect or sort | Max-heap of size K |
| **Kth Smallest** | QuickSelect or sort | Min-heap of size K |

---

## 1. Kth Largest/Smallest in Array

### Approach A — Sort (Baseline)

```cpp
int kthLargest(vector<int>& nums, int k) {
    sort(nums.begin(), nums.end());
    return nums[nums.size() - k];   // or nums[k-1] for kth smallest
}
```

**Time:** O(N log N) | **Space:** O(1)  
Only useful as a sanity baseline. Never propose in interview.

---

### Approach B — QuickSelect (Optimal for Array)
![[QuickSelect]]

**Time:** O(N) average, O(N²) worst | **Space:** O(log N) recursion stack  

---

## 2. Kth Largest in a Stream

### Why QuickSelect Fails Here
A stream gives you elements one at a time. You can't partition what you haven't seen yet.

### Approach — Min-Heap of Size K
![[Kth largest using Heap]]

---

### Kth Smallest in a Stream — Max-Heap of Size K
![[Kth smallest using Heap]]
