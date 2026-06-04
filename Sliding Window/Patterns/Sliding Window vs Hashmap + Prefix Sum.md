
| Element Sign             | Problem Type                      | Condition    | Best Tool                 |
| ------------------------ | --------------------------------- | ------------ | ------------------------- |
| **>= 0** (non-negatives) | Any (`Min/Max` length or `Count`) | `<=` or `>=` | Sliding Window            |
| **>= 0** (non-negatives) | `Count`                           | `== k`       | `count(≤k) - count(≤k-1)` |
| **>= 0** (non-negatives) | `Min/Max` length                  | `== k`       | Sliding Window            |
| **Any Integer**          | `Count` or `Min/Max` length       | `== k`       | Prefix Sum + HashMap      |
