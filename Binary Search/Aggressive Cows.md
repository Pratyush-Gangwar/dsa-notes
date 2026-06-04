### Why sort?
- We sort because its easier to reason about position in ascending order.

### Predicate
Try to greedily place at least $k$ cows such that the distance b/w each is $minDist$

### Direction
Initially, when $minDist$ is small, you'll be able to place much more than $k$ cows but as $minDist$ increases, you'll eventually be unable to place even $k$ cows. So, the direction is $yes-no$ and we're looking for the last $yes$.