## Modifying CountInversions is wrong!
```cpp
class Solution {
public:
    int merge(int lo, int mi, int hi, vector<int>& nums) {
        vector<int> temp;

        int p1 = lo;
        int p2 = mi + 1;

        int count = 0;

        while (p1 <= mi && p2 <= hi) {
            if (nums[p1] <= nums[p2]) {
                temp.push_back( nums[p1] );
                p1++;
            }

            else if (nums[p1] > nums[p2]) {
                if (nums[p1] > 2 * nums[p2]) count += (mi - p1 + 1);

                temp.push_back( nums[p2] );
                p2++;
            }
        }

        while (p1 <= mi) {
            temp.push_back( nums[p1] );
            p1++;
        }

        while (p2 <= hi) {
            temp.push_back( nums[p2] );
            p2++;
        }

        for(int i = lo; i <= hi; i++) {
            nums[i] = temp[i - lo];
        }

        return count;
    }

    int helper(int lo, int hi, vector<int>& nums) {
        if (lo == hi) {
            return 0;
        }

        int mi = lo + (hi - lo)/2;

        int leftCount = helper(lo, mi, nums);
        int rightCount = helper(mi + 1, hi, nums);
        int crossCount = merge(lo, mi, hi, nums);

        return leftCount + rightCount + crossCount;
    }

    int reversePairs(vector<int>& nums) {
        return helper(0, nums.size() - 1, nums);
    }
};
```

The reason this doesn't work is that the merge condition `arr[p1] > arr[p2]` is different from the reverse-pair condition `arr[p1] > 2 * arr[p2]`. This leads to mistakes. 

Note that for count inversions, the merge and inversion condition are exactly the same! So, we can count during the while loops. 

Example: `left = [6, 13], right = [4, 5]`. The above merge algorithm will return 0 as the count.

## So, what do we do?
Well, if we can't count inside the while loops, we'll count outside it. We can't count after it because all the reverse pairs will be gone (arrays will be merged in sorted order). So, we count before it. 

```cpp
for(int i = lo; i <= mi; i++) {
    while (j <= hi && nums[i] > 2LL * nums[j]) j++;
    count += ( (j - 1) - (mi + 1) + 1);
}
```

For each `nums[i]`, we try to find the count of reverse pairs by finding the largest `j` which satisfies `nums[i] > 2 * nums[j]`. Since both arrays are sorted, we can count reverse pairs using the given formula. We add this up across all `i` values. 

Note that `j` itself is excluded form the count.

Note that `j` is not reset across iterations. Say your previous values were `i1, j1`. We know that `arr[i1] > arr[j], for all j in [mi + 1, j1 - 1]`. Now, consider `i2 = i1 + 1`. Since `arr[i2] >= arr[i1]`, so `arr[i2] > arr[j], for all j in [mi + 1, j1 - 1]`. So, it makes no sense to reset `j` every iteration. Rather, we continue where we left off. 

Technically, this approach works for count inversions too. But it requires an extra pass. The traditional count inversions algorithm is actually an optimization of this approach to use just one pass. 

## Edge cases
### If `j = mi + 1` at the end of the while-loop for some `i`
This happens when `nums[i] <= 2 * nums[mi + 1]` for some `i`. So, no reverse pairs are possible.
Then, the count added will be `(mi + 1 - 1) - (mi + 1) + 1 = 0`.

### If `j = hi + 1` at the end of the while-loop for some `i`
This happens when `nums[i] > 2 * nums[hi]` for some `i`. So, the entire right array can form a reverse pair with that `i` value.

Then, the count added will be `(hi + 1 - 1) - (mi + 1) + 1 = hi - mi`.

## Deriving it yourself
![[Pasted image 20260524183749.png]]

Try to make list out all the `j`'s possible for each `i`.
You will notice that each `i` appends elements to the previous `i`'s list!

So, we don't need nested loops. We can keep a counter outside the `while` loop for the right list and keep modifying that. 