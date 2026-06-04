## Naive Approach
### Attempt 1
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;

        for(int i = 0; i < nums.size(); i++) {
            for(int j = 0; j < nums.size(); j++) {
                for(int k = 0; k < nums.size(); k++) {
                    if (i == j || j == k || k == i) {
                        continue;
                    }

                    if (nums[i] + nums[j] + nums[k] == 0) ans.push_back(vector<int> {nums[i], nums[j], nums[k]} );
                }
            }
        }
        return ans;
    }
};
```

For a SET (not ordering) of indices {i, j, k}, this code pushes all 3! = 6 permutations of it into the `ans` vector. 

**Why?** Consider some set `{i1, j1, k1}`. One possible permutation formed by the loops is `i = i1, j = j1, k = k1`. Another possible permutation formed by the loops is `i = j1, j = i1, k = k1`. 

### Attempt 2 (No Duplicates)
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;

        for(int i = 0; i < nums.size(); i++) {
            for(int j = i + 1; j < nums.size(); j++) {
                for(int k = j + 1; k < nums.size(); k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) ans.push_back(vector<int> {nums[i], nums[j], nums[k]} );
                }
            }
        }

        return ans;
    }
};
```

Now, we enforce an ordering `i < j < k`. So, for any set of indices `{i1, j1, k1}`, the ONLY permutation that can be formed by the loops is `i1 < j1 < k1`. 

Notice we got rid of the `i == j || j == k || k == i` condition because it is no longer needed.

This would've have been enough if the array had no duplicates.
### Attempt 3 (Duplicates)
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        set<vector<int>> ans;

        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                for (int k = j + 1; k < nums.size(); k++) {

                    if (nums[i] + nums[j] + nums[k] == 0) {

                        vector<int> triplet = {
                            nums[i],
                            nums[j],
                            nums[k]
                        };

                        sort(triplet.begin(), triplet.end());

                        ans.insert(triplet);
                    }
                }
            }
        }

        return vector<vector<int>>(ans.begin(), ans.end());
    }
};
```

The array can have duplicates, for example, `[-1, 1, 0, -1]`. So, `i = 0, j = 1, k = 2` and `i = 1, j = 2, k = 3` both satisfy the index ordering rule. But their *set* of values is identical, i.e., `{-1, 1, 0}`. The question forbids this. 

So, we utilize `set<vector<int>>` to keep track of used values. We sort the vector before storing to make sure that `[-1, 1, 0]` and `[-1, 0, 1]` are treated as duplicates. Note, `unordered_set<vector<int>>` is not valid syntax.
 
**Why not `set<set<int>>` or `set<unordered_set<int>>`? Wouldn't that automatically take care of making suring order is irrelevant while matching dupes?** The inner `set<int>` or `unordered_set<int>` would only store unique values. So, `{-1, -1, 2}` would collapse to `{-1, 2}` which is wrong! So, we must use a `vector<int>`.

Since we're using a `set`, we could've skipped the index ordering rule. But it would increase the run-time by a factor of `6`. You would also need to readd the `i == j || j == k || k == i` check.

Run time is `N^3 log K` where `K` is the number of answers. 
Space complexity is `2 * K` (one for set, one for returned vector).

## The Key Idea
If you fix `nums[i]`, then finding `nums[j]` and `nums[k]` boils down to 2Sum. 
We have two approaches to do this - the set based approach and the two pointer approach.


## Set Approach (No Duplicates)
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;

        for(int i = 0; i < nums.size(); i++) {
            unordered_set<int> seen;

            for(int k = i + 1; k < nums.size(); k++) {
                int left = -1 * (nums[i] + nums[k]);

                if (seen.find( left ) != seen.end()) {
                   vector<int> v = { nums[i], left, nums[k] };
                   ans.push_back(v); 
                }

                else {
                    seen.insert( nums[k] );
                }
            }

        }

        return ans;
    }
};
```

For `i`, we apply 2Sum to `[i + 1, n)`. Notice carefully that the inner index is `k` and not `j`. This is because for every `k` value, the set stores numbers in the range `[i+1, k-1]` which is `j`. 

We enforce indexing order so that different permutations of the same set of indices `{i1, j1, k1}` is not pushed twice. This is enough if the array has no duplicates.

Runtime is `O(N^2)`. Space is `O(K + N)` since the `unordered_set` is re-initialized each iteration.

## Set Approach (Duplicates)
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        set<vector<int>> ans;

        for(int i = 0; i < nums.size(); i++) {
            unordered_set<int> seen;

            for(int k = i + 1; k < nums.size(); k++) {
                int left = -1 * (nums[i] + nums[k]);

                if (seen.find( left ) != seen.end()) {
                   vector<int> v = { nums[i], left, nums[k] };
                   sort(v.begin(), v.end());
                   ans.insert(v); 
                }

                else {
                    seen.insert( nums[k] );
                }
            }

        }

        return vector<vector<int>> (ans.begin(), ans.end());
    }
};
```
We do not really need to enforce the indexing order here since we use a set anyway. But it would increase the run-time by a factor of `6`. You would also need to readd the `i == j || j == k || k == i` check.

You may note that since we are using an `unordered_set<int>`, any duplicates in `[i + 1, k - 1]` will be collapsed. This is fine. 

Runtime is `O(N^2 log K)`. Space is `O(2K + N)`.

## Further Optimization
We want to remove the `K` extra space used by the set. 

## 2 Pointer Approach (No Duplicates)
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;
        sort(nums.begin(), nums.end());

        for(int i = 0; i < nums.size(); i++) {
            int target = 0 - nums[i];

            int j = i + 1;
            int k = nums.size() - 1;

            while (j < k) {
                int val = nums[j] + nums[k];
                if (val == target) {
                    ans.push_back( vector<int> { nums[i], nums[j], nums[k] } );
                    j++;
                    k--;
                }
                
                else if ( val > target ) {
                    k--;
                }

                else if ( val < target ) {
                    j++;
                }
            }
        }

        return ans;
    }
};
```

- We sort to array to use the two point method.
- We initialize `j = i + 1, k = n - 1` and keep the while-loop condition as `j < k` to ensure index ordering. 
- The while loop condition is strict `j < k` since `j == k` would point to the same element instead of two distinct elements. 

- If `val > target`, we do `k--` so that the sum can reduce. 
- If `val < target`, we do `j++` so that the sum can increase.
- If `val == target`, why would we `j++, k--`?
	- Say we only did `j++`, then the sum would increase beyond `target` for sure. So, we don't do this.
	- Say we only did `k--`, then the sum would decrease below `target` for sure. So, we don't do this. 
	- If we do both `j++, k--`, then the sum has a chance of staying at `target`. 

- This approach would've been enough if the array had no duplicates. 

## 2 Pointer Approach (Duplicates)
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;
        sort(nums.begin(), nums.end());

        for(int i = 0; i < nums.size(); i++) {
            if (i != 0 && (nums[i] == nums[i - 1])) continue;

            int target = 0 - nums[i];

            int j = i + 1;
            int k = nums.size() - 1;

            while (j < k) {
                int val = nums[j] + nums[k];

                if (val == target) {
                    ans.push_back( vector<int> { nums[i], nums[j], nums[k] } );

                    j++;
                    k--;
                    
                    while (j < k && j < nums.size() && nums[j] == nums[j - 1]) j++;
                    while (j < k && k >= 0 && nums[k] == nums[k + 1]) k--;
                }
                
                else if ( val > target ) {
                    k--;
                }

                else if ( val < target ) {
                    j++;
                }
            }
        }

        return ans;
    }
};
```

Since the array is sorted, duplicates appear consecutively, for example:
```text
[-1, -1, -1, 0, 0, 1, 1, 1, 1, 2, 2]
```

We use:
```cpp
if (i != 0 && nums[i] == nums[i - 1]) continue;
```
to ensure that the two-pointer search only runs for the *first* occurrence of every value.

We do not miss any triplets by doing this. Suppose the value `1` appears multiple times. The search space after the first `1` is the largest possible. Any later occurrence of `1` would only produce a smaller subarray, so any triplet it could form would already have been considered earlier.

However, this alone does not completely eliminate duplicates. For example, after fixing `i = 0`, the pair `(-1, 1)` may still occur multiple times due to duplicates in the remaining array.

To handle this, after finding a valid triplet (`val == target`), we move both pointers once:
```cpp
j++;
k--;
```

and then skip over consecutive duplicates using `while` loops.

The initial `j++` and `k--` are necessary because even when no duplicates exist, we still need to move both pointers forward. We perform these increments *before* the duplicate-skipping loops; otherwise, the pointers would move an extra step after all duplicates had already been skipped.

Time is `O(N^2 + N log N)`. Space is `O(K)`.