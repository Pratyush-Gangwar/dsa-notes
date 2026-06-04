## Longest Consecutive Sequence
### Solution
```spoiler-markdown
We can think of the array as having various components which are internally connected but externally disjoint. 

We want to iterate over the elements and check if it is the START of a connected component. There's no point in exploring an element that is the MIDDLE of a connected component since the longest sequence will always be from the START of a connected component. 

For efficient lookup, we convert the array into an unordered set. The complexity of the solution is O(3N) because (1) conversion of array to set, (2) iterating over entire set, (3) exploring each element in a connected component. 

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> numSet(nums.begin(), nums.end());
        int maxLen = 0;

        for(int num : numSet) {
            if (numSet.find(num - 1) != numSet.end()) {
                continue;
            }

            int len = 1;
            for(int delta = 1; delta < nums.size(); delta++) {
                if (numSet.find( num + delta ) != numSet.end()) {
                    len++;
                }

                else {
                    break;
                }
            }

            maxLen = max(maxLen, len);
        }

        return maxLen;

    }

};
```

### Mistake
```spoiler-markdown
If you iterate over the ARRAY instead of the SET, then complexity will still be O(N) but you might perform additional work because of iterating over duplicates. This caused TLE on my leetcode submission.
```

## Trapping Rainwater
### Idea
```spoiler-markdown
The height of water at index i is determined by (min( suffixMax[i], prefixMax[i] ) - height[i]), if height[i] < suffixMax[i] and height[i] < prefixMax[i]. 

Note, suffix/prefix maximum do not need to calculated in a MONOTONIC fashion. Just the regular stream maximum is enough. This insight is all that is needed to not use extra O(N) space for the suffix/prefix maximum arrays. 
```

### Optimization
```spoiler-markdown
We do not actually need to know both the prefix and suffix maximums for a given index `i`. We only need to know the MINIMUM of the two. 

Consider two pointers and two maximum variables. pMax represents maximum in range [0, l - 1] for index `l`. sMax represents maximum in range [r + 1, n - 1] for index `r`. These are numbers, not arrays. 

If pMax (at l) < sMax (at r), then we can be assured that pMax (at l) < sMax (at l) since suffix maximum can only increase, never decrease. So, min(pMax (at al), sMax (at l)) = pMax (at l). So, at l, the water height is pMax (at l) - height[l]. We increment the l pointer to mark this element as processed. 

If sMax (at r) <= pMax (at l), then sMax (at r) <= pMax (at r). And similar logic applies.

We use a weak inequality in while-loop (l <= r) so that all bars can be processed.
``` 

## Merge Overlapping Intervals
```spoiler-markdown
We sort the intervals by ascending order of starting time. We maintain an array `ans` containing the final merged intervals. Each interval in the `ans` array is disjoint from each other. Therefore, while processing is happening, only the last interval in the `ans` array can be 'extended', that is, merged with another interval.

For each interval in the sorted array, we check if it overlaps with the last interval in `ans` array by checking if its starting time <= ending time of last interval in the ans array. Note, sorting by start time ensured that starting time of the current interval >= starting time of the last interval in `ans` array.

If so, we merge the two intervals by setting the ending time of the last interval in the ans array to the maximum of the ending time of the two intervals. Otherwise, we append the interval to the `ans` interval as it is disjoint.
```

## CountInversions
![[CountInversions]]

### Mistakes:
```spoiler-markdown
I incremented inversions by 1 in the first while loop instead of (mi - p1 + 1). I incremented inversions by 1 in the second while loop causing double-counting. 

```cpp
class Solution {
  public:
    int merge(int lo, int mi, int hi, vector<int>& arr) {
        vector<int> temp;
        
        int p1 = lo;
        int p2 = mi + 1;
        
        int inversions = 0;
        
        while (p1 <= mi && p2 <= hi) {
            if (arr[p1] <= arr[p2]) {
                temp.push_back( arr[p1] );
                p1++;
            }
            
            else {
                temp.push_back( arr[p2] );
                inversions++;
                p2++;
            }
        }
        
        while (p1 <= mi) {
            temp.push_back( arr[p1] );
            inversions++;
            p1++;
        }
        
        while (p2 <= hi) {
            temp.push_back( arr[p2] );
            p2++;
        }
        
        for(int i = 0; i <= hi - lo; i++) {
            arr[lo + i] = temp[i];
        }
        
        return inversions;
    }
    
    int countInversions(int lo, int hi, vector<int>& arr) {
        if (lo == hi) {
            return 0;
        }
        
        int mi = lo + (hi - lo)/2;
        
        int leftInversions = countInversions(lo, mi, arr);
        int rightInversions = countInversions(mi + 1, hi, arr);
        int crossInversions = merge(lo, mi, hi, arr);
        
        return leftInversions + rightInversions + crossInversions;
    }
  
    int inversionCount(vector<int> &arr) {
        return countInversions(0, arr.size() - 1, arr);
        
    }
};
```

## Level-Order Traversal of a Binary Tree
### Mistakes
```spoiler-markdown
- Didn't check if root was nullptr
- Didn't check if children were non-nullptr before pushing into queue. 
```

## LCA of BST, BT
```spoiler-markdown
![[LCA]]
```


## Height, Diameter
```spoiler-markdown
![[Height, Diameter]]

Mistake: I wrote the recurrence in terms of edges instead of nodes which made it hell. 
```
