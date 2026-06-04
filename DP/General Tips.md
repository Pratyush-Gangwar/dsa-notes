## Memoisation
Never memoise inside base-cases. Since base-cases don't check for state validating, you might get a SEG FAULT if you try to write to an invalid DP index. 
## Handling base and edge cases in tabulation
In order to make your life easier, don't write out separate loops to populate the base cases or conditions to handle edge cases. In fact, never READ directly from the DP array. You can write to it directly but use a helper function to read from it. This helper function is an exact copy of the memoisation base cases!

### Example
#### Memoisation
```cpp
class Solution {
  public:
    bool helper(int idx, int sum, vector<int>& arr, vector<vector<int>>& dp) {
        if (sum == 0) {
            return true;
        }
        
        else if (idx < 0) {
            return false;
        }
        
        else if (dp[idx][sum] != -1) {
            return dp[idx][sum];
        }
        
        
        bool take = false;
        if (arr[idx] <= sum) {
            take = helper(idx - 1, sum - arr[idx], arr, dp);
        }
        
        
        if (take) {
            return dp[idx][sum] = true;
        }
        
        return dp[idx][sum] = helper(idx - 1, sum, arr, dp);
    }
  
    bool isSubsetSum(vector<int>& arr, int sum) {
        // code here
        vector<vector<int>> dp(arr.size(), vector<int> (sum + 1, -1));
        return helper(arr.size() - 1, sum, arr, dp);
        
    }
};
```

#### Tabulated version
```cpp
class Solution {
  public:
    bool getValue(int idx, int sum, vector<int>& arr, vector<vector<int>>& dp) {
        if (sum == 0) {
            return true;
        }
        
        else if (idx < 0) {
            return false;
        }
        
        else {
            return dp[idx][sum];
        }
        
    }

    bool isSubsetSum(vector<int>& arr, int sum) {
        // code here
        vector<vector<int>> dp(arr.size(), vector<int> (sum + 1, -1));
        
        for(int idx = 0; idx < arr.size(); idx++) {
            for(int target = 0; target <= sum; target++) {
                
                bool take = false;
                if (arr[idx] <= target) {
                    take = getValue( idx - 1, target - arr[idx], arr, dp);
                }
                
                bool not_take = getValue( idx - 1, target, arr, dp);
                
                dp[idx][target] = take || not_take;
            }
        }
        
        return getValue(arr.size() - 1, sum, arr, dp);
        
    }
};
```

## Loop directions in tabulation
The outer loop will always be opposite the direction of your recursive call. Include the entire range. Don't worry about excluding the base case or whatever. If you're following my `readHelper` method, then you won't run into problems. 

The inner loop (for 2D DP) direction depends on whether you're accessing elements in the current row. If so, you must keep it such that no invalid accesses occur. If you're not accessing elements in the current row, then you can use either direction.