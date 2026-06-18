##  Approach 1
```cpp
class Solution {
  public:
    void markSeen(int num, vector<int>& arr) {
        arr[num - 1] *= -1;
    }
    
    bool isSeen(int num, vector<int>& arr) {
        return arr[num - 1] < 0;
    }
    
    int getNum(int idx, vector<int>& arr) {
        return abs(arr[idx]);
    }
    
    vector<int> findTwoElement(vector<int>& arr) {
        // code here
        int expectedSum = 0;
        int actualSum = 0;
        int dup;
        
        for(int i = 0; i < arr.size(); i++) {
            int num = getNum(i, arr);
            if (isSeen(num, arr)) {
                dup = num;
            }
            
            markSeen(num, arr);
            
            actualSum += num;
            expectedSum += (i + 1);
        }
        
        int missing = (expectedSum) - (actualSum - dup);
        return vector<int> {dup, missing};
    }
};
```

So, we can't use extra space but still need to store information per element. This usually implies we need to store both the original elements and information about them in the same array!

Here, we store information about number `x` at index `x - 1`, that is, `arr[x - 1]`. We do non-destructively so that the value already present at index `x - 1` is not overwritten (multiplication by -1 is reversible).

## Approach 2 (See striver)
Find `missing + repeated` and `missing - repeated`.