```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int candidate = INT_MIN;
        int count = 0;

        for(int num : nums) {
            if (count == 0) {
                candidate = num;
            }

            if (num == candidate) {
                count++;
            }

            else {
                count--;
            }
        }

        return candidate;
    }
};
```

If `num == candidate`, we do `count++`. But otherwise, we do `count--`, that is we 'cancel' a count of the candidate if a different element occurs.

The logic is that the majority element occurs $> \lfloor n/2 \rfloor$ times. So, even after cancellation with the remaining $< \lfloor n/2 \rfloor$ elements, `candidate` will equal the majority element at the end!