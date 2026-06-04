# Merge Sort Stylistic Choices

## Base Case

The base case is a singleton subarray:

```cpp
if (lo == hi)
```

not an empty range:

```cpp
if (lo > hi)
```

This keeps every recursive call representing a valid subarray.

---

## Division Style

The array is divided as:

```cpp
[lo ... mi]
[mi + 1 ... hi]
```

using:

```cpp
mergeSort(lo, mi);
mergeSort(mi + 1, hi);
```

not:

```cpp
[lo ... mi - 1]
[mi ... hi]
```

This makes the midpoint belong fully to the left half.

---

## Stability
Merge sort preserves stability by using:

```cpp
<=
```

instead of:
```cpp
<
```
during merging.

When elements are equal, the left element is taken first, preserving original relative order.

---

## Merge Structure
The merge step has three clean phases:

1. Merge while both halves have elements.
2. Flush remaining left half.
3. Flush remaining right half.

This keeps the logic predictable and symmetric.

---
## Copy Back
Merged values are written into a temporary array first, then copied back into the original array range:

```cpp
nums[i] = temp[i - lo];
```

The offset `i - lo` maps original indices back into the zero-indexed temporary buffer.  