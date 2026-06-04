![[CountInversions (ADA).pdf]]

In the first while-loop, we add n/2 - p1 + 1 to the inversion count because if `arr[p2] < arr[p1]` then its less than all elements in `[p1, mi]` and so all of them are inversions. 

Inversions are counted only during:
```cpp
while (p1 <= mi && p2 <= hi)
```

because this is the only phase where elements from both halves are actively compared. Once one half is exhausted, all possible cross-half inversions have already been processed.

The remaining loops:
```cpp
while (p1 <= mi)
while (p2 <= hi)
```

simply append leftover elements in sorted order. They are cleanup loops, not comparison loops, so no new inversions can be discovered there.