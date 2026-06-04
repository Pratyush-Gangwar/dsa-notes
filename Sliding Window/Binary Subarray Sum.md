## Code
```cpp
class Solution {
public:
    int countAtmost(vector<int>& nums, int goal) {
        if (goal < 0) return 0;

        int sum = 0;
        int count = 0;
        int left = 0;

        for(int right = 0; right < nums.size(); right++) {
            sum += nums[right];

            while (sum > goal) {
                sum -= nums[left];
                left++;
            }

            count += (right - left + 1);
        }

        return count;
    }

    int numSubarraysWithSum(vector<int>& nums, int goal) {
        return countAtmost(nums, goal) - countAtmost(nums, goal - 1);
    }
};
```

We need the `if (goal < 0)` guard clause because the problem says `goal` can equal `0` in which case `goal - 1` is `-1`. 

If `goal < 0`, then `while (sum > goal)` gets stuck forever since the array have non-negatives, the sum of which will always be greater than a negative goal. 

So, we return `0` because it's impossible to achieve a negative `goal` with non-negatives.