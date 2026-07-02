## Kth Most Frequent Element

| | **Array** | **Stream** |
|---|---|---|
| **Kth Most Frequent** | Bucket sort | Min-heap of size K |

---

## 1. Array — Bucket Sort
![[K Most Frequent Elements]]

---

## 2. Stream — Min-Heap of Size K

### Why Bucket Sort Fails Here
Bucket sort requires knowing N upfront to size the bucket array, and frequencies grow unboundedly as the stream continues.

### Approach — Min-Heap of Size K
![[K Most Frequent Elements using Heaps]]