```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if (nums2.size() < nums1.size()) return findMedianSortedArrays(nums2, nums1); // imp for correctness
    
        int n = nums1.size() + nums2.size();

        int lo = 0;
        int hi = nums1.size(); 

        while (lo <= hi) {
            int numLeft1 = lo + (hi - lo)/2;
            int numLeft2 = n/2 - numLeft1; // not nums1.size()

            int left1 = (numLeft1 == 0 ? INT_MIN : nums1[numLeft1 - 1] ); // why INT_MIN?
            int left2 = (numLeft2 == 0 ? INT_MIN : nums2[numLeft2 - 1] );

            int left = max(left1, left2);

            int right1 = (numLeft1 == nums1.size() ? INT_MAX : nums1[numLeft1]);
            int right2 = (numLeft2 == nums2.size() ? INT_MAX : nums2[numLeft2]);

            int right = min(right1, right2);

            if (left <= right) {
                if (n % 2 == 0) return (left + right)/2.0; // float division
                else return right;
            }

            else if (left1 > right2) {
                hi = numLeft1 - 1;
            }

            else if (left2 > right1) {
                lo = numLeft1 + 1;
            }
        }

        return 0.0;
    }
};
```

Note,
- we compare max(left1, left2) to min(right1, right2).
- we use the traditional binary search pattern (exclusive mi, inclusive hi, loop condition being lo <= hi)
- the `if (nums2.size() < nums1.size()) return findMedianSortedArrays(nums2, nums1);` line is important for ensuring that numLeft2 stays in the range `[0, nums2.size()]`. Otherwise, you may run into index errors (heap buffer overflow)!
- INT_MIN and INT_MAX are sentinel values