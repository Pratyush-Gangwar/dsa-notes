## How many such elements?
If there were $3$ such elements, each with count $> n/3$, then total count would be $> n$.

## O(N) time, O(N) space
### Two-pass
```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        unordered_map<int, int> freqs;
        for(int num : nums) freqs[num]++;

        vector<int> ans;
        for(auto [num, freq] : freqs) {
            if (freq > nums.size()/3) ans.push_back(num);
        }

        return ans;
    }
};
```

### One-pass
```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        unordered_map<int, int> freqs;
        vector<int> ans;

        for(int num : nums) {
            freqs[num]++;
            if (freqs[num] == nums.size()/3 + 1) ans.push_back(num);
        }

        return ans;
    }
};
```
The `if` condition is required to prevent the same number being added multiple times.

## Optimal Algorithm
```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int elem1 = INT_MIN;
        int elem2 = INT_MIN;

        int cnt1 = 0;
        int cnt2 = 0;

        for(int num : nums) {
            if (cnt1 == 0 && num != elem2) {
                elem1 = num;
            }

            else if (cnt2 == 0 && num != elem1) {
                elem2 = num;
            }

            if (num == elem1) {
                cnt1++;
            }

            else if (num == elem2) {
                cnt2++;
            }

            else {
                cnt1--;
                cnt2--;
            }    
        }

        cnt1 = 0;
        cnt2 = 0;

        for(int num : nums) {
            if (num == elem1) cnt1++;
            else if (num == elem2) cnt2++;
        }

        vector<int> ans;
        if (cnt1 > nums.size()/3) ans.push_back(elem1);
        if (cnt2 > nums.size()/3) ans.push_back(elem2);

        return ans;
    }
};
```

It is exactly the same as Moore's algorithm!
We introduced two potential candidates `elem1, elem2` with corresponding counts. They are initialized to sentinel values.

Note, the candidate assignment (based on `count == 0`) is `if-else`. Because if both counts are 0, only one candidate should be assigned `num`, not both! Remember, we want *distinct* candidates. 

There is also `!= elem` guard clause. This is because for an array like `[1, 2, 2, 3, 1]`, we don't want `elem1` to be assigned `1` which is the value of `elem2`. We want distinct candidates.

Notice, how we add/cancel the counts. If the current element matches ANY of the candidates, we update the corresponding count (and don't decrement the other count). Because we assume that candidates themselves don't cancel each other. But if the current element doesn't match ANY of the candidates, we decrement (cancel) both counts.

The code after the while-loop is just for sanity checking. Because, like with Moore's, `cnt` after the initial loop isn't the actual count of the element and the algorithm only works if majority elements exist!







