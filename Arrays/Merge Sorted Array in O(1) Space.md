```cpp
class Solution {
public:
    int& getElem(int p, vector<int>& nums1, int m, vector<int>& nums2, int n) {
        if (p < m) return nums1[p];
        else return nums2[ p - m ];
    }

    void printArr(vector<int>& nums) {
        for(int num : nums) cout << num << " ";
        cout << "\n";
    }

    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int currGap = ceil( (m + n) / 2.0 );

        while (true) {
            int left = 0;
            int right = left + currGap;

            while (right < m + n) {
                int& x = getElem(left, nums1, m, nums2, n);
                int& y = getElem(right, nums1, m, nums2, n); 

                if ( x > y ) {
                    swap(x, y);
                }

                left++;
                right++;
            }

            int nextGap = ceil( currGap / 2.0 );
            if (nextGap == 1 && currGap == 1) break;
            currGap = nextGap;
        }

        printArr(nums1);
        printArr(nums2);
    }
};
```

### Can swap operate between arrays?
Yes!
### Why references?
swap operates on references. 

If we were to pass `int x = arr[p1]` and `int y = arr[p2]` to swap, then it would swap `x` and `y` but not `arr[p1]` and `arr[p2]` because `x` is separate from `arr[p1]` (it's just a copy) and similarly for `y`. 

But if we were to pass `int& x = arr[p1]` and `int& y = arr[p2]` to swap, then it would swap `arr[p1]` and `arr[p2]` because `x` and `y` are aliases for `arr[p1]` and `arr[p2]` respectively. They are not copies!

Why is this important? Well, we want to use an utility function `getElem` to automatically get the correct element from our virtual indices `p1, p2`. If it returns a `int&`, we can directly swap them. If it returns `int`, we won't be able to. 