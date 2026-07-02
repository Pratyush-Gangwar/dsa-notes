## General Pattern
**Problem:** You have resources with different _flexibility_ — how many distinct demands each resource can satisfy. Demands arrive in sequence. At each demand, consume resources to fulfill it. Return whether all demands can be fulfilled.

Strict flexibility ordering means for any two resources A and B where A is more flexible, A can satisfy every demand B can, plus more.

Weak flexibility (that is, if two resources are exactly equal) or absence of an ordering (arbitrary coin systems) requires DP.
## Code
```cpp
bool lemonadeChange(vector<int>& bills) {
    int five = 0, ten = 0;
    for (int bill : bills) {
        if (bill == 5) { five++; }
        else if (bill == 10) { five--; ten++; }
        else { // bill == 20
            if (ten > 0) { ten--; five--; } // spend $10 first — less flexible than $5
            else { five -= 3; }
        }
        if (five < 0) return false;
    }
    return true;
}
```