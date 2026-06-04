## Directly using the template
```cpp
int left = 0;
int windowSum = 0;
int maxLen = 0;

for (int right = 0; right < n; ++right) {
    // expand window
    windowSum += nums[right];

    // shrink while invalid (sum exceeds target)
    while (windowSum > target) {
        windowSum -= nums[left];
        left++;
    }

    maxLen = max(maxLen, right - left + 1);
}
```

Note, we do not need the `if (cond) update` here since after the while loop, the condition will always be valid. 

Note, we cannot replace the `while` loop with an `if` here since the `windowSum += nums[right]` changes the sum by `> 1`.