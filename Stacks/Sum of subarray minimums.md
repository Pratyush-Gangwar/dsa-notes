## The problem with using `>=` in both PSE and NSE
### Setup
Imagine an array where a local minimum value X appears at index i and index j (where i<j).
- Every element between index i and index j is strictly greater than X.
- To the left of i, the first element that is **smaller** than X is at index L. (If no such element exists, L=−1).
- To the right of j, the first element that is **smaller** than X is at index R. (If no such element exists, R=array length).

```
Indices:   ...   L   ...   i   ...   j   ...   R   ...
Array:     ...  [A]  ...  [X]  ...  [X]  ...  [B]  ...
```

### Range for index i:
- **PSE (Left):** Looks left, skips the `>=` elements, and stops at index L. `pse[i]=L`
- **NSE (Right):** Looks right, skips the `>=` elements, and stops at index R. `nse[i]=R`
- **Total Territory:** Any subarray starting from L+1 to i, and ending from i to R−1.

### Range for index j:
- **PSE (Left):** Looks left, skips the `>=` elements, and stops at index L. `pse[j]=L`
- **NSE (Right):** Looks right, skips the `>=` elements, and stops at index R. `nse[j]=R`
- **Total Territory:** Any subarray starting from L+1 to j, and ending from j to R−1.

### Double Counting
Consider any subarray `[s, e]` (with `s > L`, `e < R`) which includes `[i, j]`.

Because both `i` and `j` have the exact same boundaries, both i and j will look at this subarray, see that it falls within their calculated range, and add X to the total sum. You have double-counted the minimum for this subarray.

## The Fix
So, if a subarray has multiple (equal) minimum elements, we need a mathematical rule that forces **exactly one** of these indices to claim the subarray. 

For example, we want that only the first (equal) minimum element will count the subarray and the others will not. 

For this, **`getNSE` uses `>=`**  and `getPSE` uses `>`.

### Range for index i:
- **PSE (Left):** Looks left, skips the `>` elements, and stops at index L. `pse[i]=L`
- **NSE (Right):** Looks right, skips the `>=` elements, and stops at index R. `nse[i]=R`
- **Total Territory:** Any subarray starting from L+1 to i, and ending from i to R−1.

### Range for index j:
- **PSE (Left):** Looks left, skips the `>` elements, and stops at index L. `pse[j]=i`
- **NSE (Right):** Looks right, skips the `>=` elements, and stops at index R. `nse[j]=R`
- **Total Territory:** Any subarray starting from i+1 to j, and ending from j to R−1.

### No more double counting
Consider any subarray `[s, e]` (with `s > L`, `e < R`) which includes `[i, j]`.

This is counted by `i` since it starts from L+1 to i, and ends from i to R−1.
This is NOT counted by `j` since `s <= i` but `j` requires starting from `i + 1` to `j`.