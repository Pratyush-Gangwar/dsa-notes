## Attributes and Objects
We can say that the attributes of a stone are its row and column. 
If two stones share either or both of these attributes, they should be connected. 
This is the classic `Union over attributes, not objects` pattern. 

## Distinguishing rows and columns
Since both are 0-indexed, we use replace `col` by `10001 + col` (`row` can be `10001` at most). This way we do not have to find the maximum row manually. 

## Counting
We know that the answer is the `number of stones in each component - 1` summed over all components. But as you know, `size[x]` (where `x` is the parent of a connected component) does not represent the number of stones in that component but rather the number of attributes in that component. 

Luckily for us `sum(number of stones in each component - 1) = total stones - number of connected components`. 