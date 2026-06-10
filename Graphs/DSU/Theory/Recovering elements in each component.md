## Two Types
**At the end** — you only need component membership after all unions are complete. You do a single pass at the end to bucket elements by root.

**At any time** — you need to query component contents mid-union-sequence, before all unions are done. You maintain the grouping structure incrementally alongside every insert and union operation.

## At the end
### Unsorted
```cpp
unordered_map<int, vector<int>> groups;
for (auto& [elem, _] : dsu.parent)
    groups[dsu.find(elem)].push_back(elem);
```

Complexity: O(n)
### Sorted
```cpp
unordered_map<int, vector<int>> groups;
for (auto& [elem, _] : dsu.parent)
    groups[dsu.find(elem)].push_back(elem);
    
for (auto& [root, vec] : groups)
    sort(vec.begin(), vec.end());
```

Complexity: O(n log n)
## At any time
### Unsorted
CODE WILL BE ADDED WHEN I ACTUALLY ENCOUNTER THIS PROBLEM

Complexity: `O(n log n)`

### Sorted
CODE WILL BE ADDED WHEN I ACTUALLY ENCOUNTER THIS PROBLEM

Complexity: `O(n log^2 n)`
