## Directly using the template
```cpp
int left = 0;
int windowSum = 0;
int maxSum = INT_MIN;

for (int right = 0; right < n; ++right) {
    // expand window
    windowSum += nums[right];

    // shrink if window is too large
    while (right - left + 1 > k) {
        windowSum -= nums[left];
        left++;
    }

    // only update when window is exactly size k. initially, window size is less than k
    if (right - left + 1 == k) {
        maxSum = max(maxSum, windowSum);
    }
}
```

We can replace the `while` with an `if` since it would run once anyway. 