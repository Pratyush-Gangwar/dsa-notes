**Problem:** given a set of intervals, select the maximum number of them such that no two selected intervals overlap.

**Greedy approach:** sort by **end time**. Iterate through the sorted intervals, maintaining `lastEnd` (end time of the most recently selected interval). For each interval, if `interval.start >= lastEnd`, select it and update `lastEnd = interval.end`; otherwise skip it.

**Why not start time or duration sorting?**
![[Pasted image 20260630085956.png]]

https://www.cs.princeton.edu/~wayne/kleinberg-tardos/pearson/04GreedyAlgorithms-2x2.pdf

## Code
```cpp
int maxNonOverlapping(vector<pair<int,int>>& intervals) {
    sort(intervals.begin(), intervals.end(),
         [](auto& a, auto& b) { return a.second < b.second; });
    int count = 0, lastEnd = INT_MIN;
    for (auto& [start, end] : intervals) {
        if (start >= lastEnd) {
            count++;
            lastEnd = end;
        }
    }
    return count;
}
```