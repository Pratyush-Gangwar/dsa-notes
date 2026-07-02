### Greedy choice
Sort jobs by profit descending. For each job, greedily place it in the **latest free slot ≤ its deadline**. If there is no slot free, skip that job.

**Why sort by decreasing profit order?** If both a higher-profit and lower-profit job compete for the same slot, then the higher-profit job should never lose a slot to a lower-profit job. 

**Why place each job as late as possible?** An early slot might be the _only_ slot some job with a lower profit down the line could fit into (it has a tighter deadline), but since you're processing in profit order (and not deadline order) you don't know that yet — reserving the latest possible slot minimizes the damage to slots that earlier deadlines depend on.

### DSU
Naively, for each job you'd linearly scan `deadline, deadline-1, ..., 1` for a free slot — O(n) per job, O(n²) total. DSU compresses this to near O(1) amortized by making each occupied slot "point to" the nearest free slot to its left.

If `parent[i] == i`, slot `i` is free. Otherwise, `parent[i]` is just the next slot to check. But there is no guarantee it's free — it may itself be occupied, in which case it has its own pointer further left. What is guaranteed is that everything strictly between `parent[i]` and `i` is occupied, since that range was already walked and rejected by some earlier call.

Because `parent[i]` isn't guaranteed free, `find` can't just return it directly — it has to recurse via `find(parent[i])`, walking left until it hits a slot where `parent[x] == x`. 

Path compression then does `parent[x] = find(parent[x])`, rewriting `parent[x]` to point straight at that free slot. This is safe because everything skipped over on the way was already known to be occupied, so no information is lost — future `find` calls on this node just resolve in one hop instead of retracing the chain.

There's no union-by-rank or group semantics here — nothing is conceptually being merged. It's purely a compressible "next free slot to the left" pointer.

Separately, `occupy(x)` is what plants these pointers in the first place. It's only ever called right after `find(x)` has confirmed `parent[x] == x`, i.e. the slot was free. It then sets `parent[x] = x - 1`, which is safe even though `x - 1` isn't guaranteed free — the next `find` that lands there will sort that out via the recursion above.

Slot `0` is a sentinel that's never occupied, so `parent[0]` always stays `0`. If every real slot up to some deadline is occupied, the chain of pointers walks all the way down to `0`, and `find` returns `0`, which the caller checks for and skips — this is what makes the recursion terminate instead of running off the end.

### Code
```cpp
class DSU {
public:
  vector<int> parent;

  DSU(int n) {
    for(int i = 0; i <= n; i++) parent.push_back(i); // 1-based indexing
  }

  int find(int x) {
    if (parent[x] == x) return x;
    return parent[x] = find(parent[x]);
  }

  void occupy(int x) {
    parent[x] = x - 1;
  }
};

class Solution {
  public:
    vector<int> jobSequencing(vector<int> &deadlines, vector<int> &profits) {
        vector<pair<int, int>> jobs;
        for(int i = 0; i < profits.size(); i++) {
          jobs.push_back( {profits[i], deadlines[i]} );
        }

        sort(jobs.begin(), jobs.end(), [](auto& p1, auto& p2) {
          return p1.first > p2.first; // descending order sort
        });

        int totalProfit = 0;
        int totalJobs = 0;

        DSU dsu(deadlines.size());
        
        for(auto& job : jobs) {
          int profit = job.first;
          int deadline = job.second;

          int slot = dsu.find(deadline);
          if (slot == 0) continue; // no free slot

          dsu.occupy(slot);
          totalJobs++;
          totalProfit += profit;
        }

        return vector<int> {totalJobs, totalProfit};
    }
};
```

### Complexity
- Sort: O(n log n)
- DSU find/union with path compression only (no union by rank here, but the "always merge left" structure keeps chains short in practice): ~O(n α(n)), effectively linear
- Overall: **O(n log n)**, dominated by the sort

