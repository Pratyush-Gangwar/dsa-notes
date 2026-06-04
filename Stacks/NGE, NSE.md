## When to use each?
- If `nge[i] = j`  (assuming `arr[j] > arr[i]`, that is, strict inequality), then it is guaranteed that all elements in range `[i, ..., j - 1]` are `<= arr[i]`.
- If `nse[i] = j`  (assuming `arr[j] < arr[i]`, that is, strict inequality), then it is guaranteed that all elements in range `[i, ..., j - 1]` are `>= arr[i]`.