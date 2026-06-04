So, we can't just keep track of one maximum variable. Because, as the window shifts, the maximum also shifts. We need multiple candidates. 

Is there any ordering between the candidates? Consider `[1]` to be our candidate list.  Now, we add `2` to the list (through some unknown mechanism). Now, assume we don't encounter any other candidates. Any future window will always use `2` as the maximum! So, candidates should be stored in a decreasing fashion. That is, `1` should be removed and `2` should be added.

Should it be strictly or weakly decreasing? Weakly! Consider `[2, 2]`. Once the first `2` goes out of scope, the second `2` can be used. If we used strictly decreasing order, we'd lose out on the second `2`.

Finally, we can't have an indefinite number of candidates. We need to remove old candidates once they go out of scope. 

When should these candidates be removed? At the start of each iteration. 

How do we get the maximum for each window? Just look at end opposite to the insertion end. 

So, we need a data structure that supports insertion and deletion at one end, and deletion at the other end. This is a deque!
