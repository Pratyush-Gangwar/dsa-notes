## The logic
The easiest way to do this question is two place two pointers with some distance b/w them and move them at the same speed.

## What distance?
When pointer `j` is at index `L - 1` (L is length of list; 0-indexed), we want `i` to be at `L - N`. Now, let's shift these so that `i = 0`. `j` will be `L - 1 - (L - N) = N - 1`. That is, we need to have a distance of `N` nodes.