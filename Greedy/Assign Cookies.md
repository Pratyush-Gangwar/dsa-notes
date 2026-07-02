## Pattern
- **Resources**
- **Demands** with some threshold
- Goal: maximize number of satisfied demand-resource pairs (each used at most once)

The greedy insight is always the same: sort both, match the smallest resource that satisfies the smallest unsatisfied demand.
## Code
```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());

        int i = 0, j = 0;
        int count = 0;

        while (i < g.size() && j < s.size()) {
            if (s[j] >= g[i]) {
                count++;
                i++; j++;
            }

            else {
                j++;
            }
        }

        return count;
    }
};
```