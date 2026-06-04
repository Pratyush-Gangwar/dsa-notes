## Remove Duplicates From Sorted Array - O(1) space solution
```spoiler-markdown
```cpp
    int removeDuplicates(vector<int>& nums) {
        int write = 1;

        for(int read = 1; read < nums.size(); read++) {
            if (nums[read] > nums[write - 1]) {
                nums[write] = nums[read];
                write++;
            }
        }

        return write;
    }
```

## Move zeros to end - O(1) space solution
```spoiler-markdown
```cpp
    void moveZeroes(vector<int>& nums) {
        int write = 0;

        for(int read = 0; read < nums.size(); read++) {
            if (nums[read] != 0) {
                swap(nums[write], nums[read]);
                write++;
            }
        }
    }
```

## The Pattern
```spoiler-markdown
Whenever the question asks us to perform some operation on an array in-place (that is, O(1) space), then read-write two-pointer pointer should be considered. 

The writer is where you insert elements. The reader scans the array once. It is usually beyond at or beyond the writer. 

The invariant is that all elements in the range `[0, writer - 1]` satisfy the property to be maintained. Elements beyond the writer are to be processed. At the end of the loop, the elements beyond writer will be stale.  

In some cases, you might want to use `swap` instead of copy. For example, in remove duplicates from sorted array, the tail of the array doesn't matter and so copy is fine. But the tail matters for move zeros to end.  

```cpp
int writer = <INITIAL VALUE>;

for(int reader = <INITIAL VALUE>; reader < n; reader++) {
    if(element should be kept) {
        nums[writer] = nums[reader]; 
        writer++;
    }
}

```

## Rearrange Elements by Sign
```spoiler-markdown
Problems which require rearranging arrays into alternating patterns can't be solved in O(1) space if you want time to be O(N). 

Usually, we try to solve it in O(N) time, O(N) space but just one-pass over the array instead of using two passes (one to populate two temp arrays for each alternating type and second to populate the answer array). 

This can be done by keeping tracking of the number of positives/negatives so far and sending them to the appropriate index. 

```cpp
    vector<int> rearrangeArray(vector<int>& nums) {
        vector<int> ans(nums.size());

        int pos = 0;
        int neg = 1;

        for(int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) {
                ans[pos] = nums[i];
                pos += 2;
            }

            else {
                ans[neg] = nums[i];
                neg += 2;
            }
        }

        return ans;
    }
```