## Boundary Conditions are important
- Mistake: If stack is empty, I set both NSE and PSE to `-1`. So, my code failed on `[1, 1]`.
- Instead, NSE should be set to `n` and PSE to `-1`. 

## Does `>=` vs `>` matter for PSE/NSE?
It matters in `Sum of subarray minimums` but does it matter here?

- **Sum of Subarray Minimums:** You are **summing** contributions. Overlapping boundaries mean you double-count the same subarrays, corrupting the total. You _must_ use strict `>` on one side as a tie-breaker to cleanly divide the territories.
- **Largest Rectangle in Histogram:** You are finding a **maximum** (`max()`). Overlapping boundaries just mean multiple bars calculate the exact same rectangle. Redundant calculations are completely harmless because `max(10, 10)` is still `10`. So, you can use `>=`.