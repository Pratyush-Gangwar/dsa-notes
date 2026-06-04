## Template for max len satisfying *at most* condition
```cpp
int left = 0;
for (int right = 0; right < n; ++right) {
    add(nums[right]);  // expand window

    // shrink while invalid
    while (conditionIsViolated()) {
        remove(nums[left]);
        left++;
    }

	updateAnswer(left, right);
}
```

You want the **largest** window, so you only shrink when you _must_ (violation). After the while, the window is guaranteed valid — every such window is a candidate, so you update unconditionally outside.

### Replacing `while` with `if`
**You can replace `while` with `if` only if you’re certain that the condition can be violated by at most **one step** when you move `right` forward**. (i.e., adding one element to the window can break the condition by at most 1).

It does not matter if incrementing left by one does not fix the violated condition. The guarantee is that, if incrementing by right violates the condition by just one 1, then left will eventually be valid. 

Note, that you cannot skip the `if (conditionSatisfied)` in case you replace `while` with `if`.

### Example where `while` is necessary:
- If your window condition depends on **something cumulative that can jump by more than 1** (like sum of elements exceeding a threshold), a single increment of `right` could violate the condition by multiple units.
- Then `while` is needed to shrink the window repeatedly until the condition is satisfied.

## Template for min len satisfying *at least* condition
```cpp
int left = 0;
for (int right = 0; right < n; ++right) {
    add(nums[right]);

    while (condSatisfied()) {
        updateAnswer(left, right);  // record before shrinking
        remove(nums[left]);
        left++;
    }
}
```

You want the **smallest** window, so you shrink as aggressively as possible. You update _inside_ because each shrink might produce a smaller valid window — you record it before removing the left element, since after removal the window may no longer satisfy the condition.

You can't really replace `while` with `if` here. 

## Template for max len satisfying *equality* condition
```cpp
left = 0, sum = 0, maxLen = 0

for right in range(n):
    sum += arr[right]
    
    while sum > target:
        sum -= arr[left]
        left++
    
    if sum == target:
        maxLen = max(maxLen, right - left + 1)
```


## Count subarrays satisfying *at most* condition
```cpp

int left = 0, curr = 0, ans = 0;
for (int right = 0; right < n; ++right) {
    add(nums[right]);

    while (condViolated()) {
        remove(nums[left]);
        left++;
    }

    ans += right - left + 1;
}
```

We know `[left, right]` is valid. Condition is *at most* and elements are non-negative. So, any subarray of `[left, right]` is also valid. But since `right` is the main pointer, we consider subarrays ending at `right`, that is, `[left, right], [left + 1, right], ..., [right, right]`.

You can't replace `while` with `if`, it doesn't work.

## Count subarrays satisfying *equality* condition
`count(== k) = count(<= k) - count(<= k-1)`

Be careful if `k` can equal `0` because then `k - 1 = -1`. You will need a guard clause in `count <= k` to handle this.

## Count subarrays satisfying *at least* condition
```cpp
int countAtLeast(vector<int>& arr, int k) {
    int n = arr.size(), left = 0, count = 0;
    for (int right = 0; right < n; right++) {
        add(arr[right]);
        while (conditionMet(k)) {
            count += n - right;
            remove(arr[left++]);
        }
    }
    return count;
}
```

We know `[left, right]` is valid. Condition is *at least* and elements are non-negative. So, any right-extension of `[left, right]` is also valid. We consider subarrays ending at `right`, that is, `[left, right], [left, right + 1], ..., [right, n -1]`.


You can't replace `while` with `if`, it doesn't work.