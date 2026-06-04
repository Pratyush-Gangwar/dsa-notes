## Unsorted array
For each index `i`, we're looking for a partner in `[0, i - 1]`.

## Sorted array
- The while loop condition is strict `j < k` since `j == k` would point to the same element instead of two distinct elements. 

- If `val > target`, we do `k--` so that the sum can reduce. 
- If `val < target`, we do `j++` so that the sum can increase.
- If `val == target`, why would we `j++, k--`?
	- Say we only did `j++`, then the sum would increase beyond `target` for sure. So, we don't do this.
	- Say we only did `k--`, then the sum would decrease below `target` for sure. So, we don't do this. 
	- If we do both `j++, k--`, then the sum has a chance of staying at `target`. 