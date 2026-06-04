```cpp
class Solution {
public:
    bool check(int maxSum, vector<int>& nums, int k) {
        int arrs = 0;
        int sum = 0;

        for(int num : nums) {
            if (sum + num > maxSum) {
                sum = num;
                arrs++;
            }

            else {
                sum += num;
            }
        }

        arrs++; 
        return arrs <= k;
    }

    int splitArray(vector<int>& nums, int k) {
        int lo = *max_element(nums.begin(), nums.end());
        int hi = accumulate(nums.begin(), nums.end(), 0) + 1;

        while (lo < hi) {
            int mi = lo + (hi - lo)/2;

            if (check(mi, nums, k)) {
                hi = mi;
            }

            else {
                lo = mi + 1;
            }
        }

        return lo;
    }
};
```

### Predicate
Greedily try to divide array into at most $k$ subarrays such that each has sum at most equal to $maxSum$

**Why at most $k$?** If we have exactly $k$ subarrays, we're done. If we have $< k$ subarrays, we can always split some of the subarrays into smaller subarrays to get $k$ subarrays, each satisfying $sum < maxSum$.

Note, if we have more than $k$ subarrays, then we cannot merge some of them to get $k$ subarrays because the sum of the merged subarray might exceed $maxSum$!
### Direction
Initially, when $maxSum$ is small, we'll get much more than $k$ subarrays. As $maxSum$ increases, we'll eventually get $\leq k$ subarrays. So, the direction is $no-yes$ and we're looking for the first $yes$.