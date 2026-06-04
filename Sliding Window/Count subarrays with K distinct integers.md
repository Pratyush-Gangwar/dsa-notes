```cpp
class Solution {
public:
    int helper(vector<int>& nums, int k) {
        int count = 0;
        int left = 0;
        unordered_map<int, int> freqs;

        for(int right = 0; right < nums.size(); right++) {
            freqs[ nums[right] ]++;

            while ( freqs.size() > k ) {
                freqs[ nums[left] ]--;
                if ( freqs[nums[left]] == 0 ) freqs.erase( nums[left] );
                left++;
            }

            count += (right - left + 1);
        }

        return count;
    }

    int subarraysWithKDistinct(vector<int>& nums, int k) {
        return helper(nums, k) - helper(nums, k - 1);
    }
};
```