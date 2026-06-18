### Finding the k-th Node from the End (Two Pointers)

We want `left` to stop at the k-th node from the end.

Suppose the list length is `n`.

Initially:

```
left  = 0
right = ?
```

When the algorithm finishes, `right` is at the last node:

```
right = n - 1
```

At that moment, `left` should be at the k-th node from the end:

```
left = n - k
```

Since both pointers move together after initialization, the distance between them never changes.

Therefore the required gap is:

```
(right final) - (left final)
= (n - 1) - (n - k)
= k - 1
```

So we need:

```
right - left = k - 1
```

at the start as well.

Since `left` starts at index `0`:

```
right = k - 1
```

To place `right` at index `k - 1`, we move it `k - 1` steps from the head.

Starting from index `0`:

```
0 --(1 step)--> 1
```

One step increases the index by 1.

Therefore:

```
k - 1 steps
```

place `right` at:

```
0 + (k - 1)
= k - 1
```

which creates the exact gap needed for `left` to eventually stop at index:

```
n - k
```

(the k-th node from the end).