## General Pattern
**Problem:** Given items with (benefit, cost) and budget C for cost, maximize total benefit.
**Algorithm:**
1. Compute ratio = benefit/cost for each item.
2. Sort descending by ratio.
3. Take items whole until one doesn't fit, then take the fraction that fills remaining budget.

Discrete case requires DP.
## Code
```cpp
double fractionalKnapsack(int C, vector<pair<int,int>>& items) {
    // {benefit, cost}
    sort(items.begin(), items.end(), [](auto& a, auto& b) {
        return (double)a.first/a.second > (double)b.first/b.second;
    });
    double total = 0;
    for (auto& [b, c] : items) {
        if (C >= c) { total += b; C -= c; }
        else { total += (double)b/c * C; break; }
    }
    return total;
}
```