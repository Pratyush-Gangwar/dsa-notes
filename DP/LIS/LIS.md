## O(N^2) time
The DP array is initialized to all 1's since initially, we assume each element to constitute its own subsequence of length 1. 

The outer loop is for populating the dp array. The inner loop is for candidates.

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp(nums.size(), 1);

        for(int i = 0; i < nums.size(); i++) {
            for(int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
        }

        return *max_element(dp.begin(), dp.end());
    }
};
```

## O(N log N) time using greedy + binary search
https://leetcode.com/problems/longest-increasing-subsequence/solutions/1326308/cpython-dp-binary-search-bit-segment-tre-wc3w

We start a new sequence if the current element is <= the back of the temp array. The equality is because we a strictly increasing sequence.