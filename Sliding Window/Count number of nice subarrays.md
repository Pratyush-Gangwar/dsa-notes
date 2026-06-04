## Two-pass approach
```cpp
class Solution {
public:
    int countAtmost(vector<int>& nums, int k) {
        int arrCount = 0;
        int oddCount = 0;
        int left = 0;

        for(int right = 0; right < nums.size(); right++) {
            if ( nums[right] % 2 == 1 ) oddCount++;

            while ( oddCount > k ) {
                if ( nums[left] % 2 == 1 ) oddCount--;
                left++;
            }

            if ( oddCount <= k ) {
                arrCount += (right - left + 1);
            }
        }

        return arrCount;
    }

    int numberOfSubarrays(vector<int>& nums, int k) {
        return countAtmost(nums, k) - countAtmost(nums, k - 1);
    }
};
```

## Relating to `Binary Subarray Sum`
Replace each `num` by `num % 2`. Then, problem is equivalent to find number of subarrays with sum `== K` in a binary array.