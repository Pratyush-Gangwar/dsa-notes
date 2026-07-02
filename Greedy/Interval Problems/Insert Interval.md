## Algorithm
The correct insert position for `newInterval` is *after* the last interval which has a start time `<= newInterval.start` since we want the array to remain sorted by start time. You *could* use binary search to find this but since you still need to do linear scans to merge the rest of the array, the overall complexity would remain O(N).

`mergeInterval` is separated out because the same merge-or-append logic is needed twice — once for `newInterval` itself, once per remaining interval. It automatically handles any nasty empty array conditions. 

## Code
```cpp
class Solution {
public:
    void mergeInterval(vector<vector<int>>& partial, vector<int>& next) {
      if (!partial.empty() && next[0] <= partial.back()[1]) {
        partial.back()[1] = max(partial.back()[1], next[1]);
      }

      else {
        partial.push_back(next);
      }
    }

    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> res;

        int last = -1;
        for (int i = 0; i < intervals.size(); i++) {
          auto& interval = intervals[i];

          if (interval[0] <= newInterval[0]) {
            last = i;
            res.push_back( interval );
          } else {
            break;
          }
        }
        
        mergeInterval(res, newInterval);

        for(int i = last + 1; i < intervals.size(); i++) {
          mergeInterval(res, intervals[i]);
        }

        return res;
    }
};
```