We will have two parameters `idx, target`.
Usually, we will have two choices - take and not take. 

Useful base-cases are:
- `if (target == 0) return true` since its always possible to form an empty subset. 
- `if (idx < 0) return false` means no elements are left but you still haven't fulfilled your target. Natural base-case for problems in which you consume elements. 

When taking an element, you should check if `arr[idx] <= target` to avoid an extra `target < 0` base-case.

DP initialization is usually N x (Sum + 1). The +1 is for the case in which sum = 0.