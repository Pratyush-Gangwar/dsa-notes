## Code
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int minLen = nums.size() + 1;
        bool found = false;

        int sum = 0;
        int left = 0;

        for(int right = 0; right < nums.size(); right++) {
            sum += nums[right];

            while (sum >= target) {
                found = true;
                minLen = min(minLen, right - left + 1);
                sum -= nums[left];
                left++;
            }
        } 

        return found ? minLen : 0;
    }
};
```