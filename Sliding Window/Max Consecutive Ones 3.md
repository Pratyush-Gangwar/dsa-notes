## Sliding window condition
Find the longest subarray that contains at most k zeros

## while-loop
```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int zeros = 0;
        int left = 0;
        int maxLen = 0;

        for(int right = 0; right < nums.size(); right++) {
            if (nums[right] == 0) zeros++;

            while (zeros > k) {
                if (nums[left] == 0) zeros--;
                left++;
            }

            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

## Replacing while loop with if
```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int zeros = 0;
        int left = 0;
        int maxLen = 0;

        for(int right = 0; right < nums.size(); right++) {
            if (nums[right] == 0) zeros++;

            if (zeros > k) {
                if (nums[left] == 0) zeros--;
                left++;
            }

            if (zeros <= k) { // this if-check cannot be removed because window may be invalid
                maxLen = max(maxLen, right - left + 1);
            }
        }

        return maxLen;
    }
};
```