## Floyd's Algorithm
Floyd's algorithm usually returns the second middle element for even-length lists and the middle element for odd-length lists. 

But, here, we want the first middle element for even-length lists and the element just before the middle element for odd-length lists. 

### How?
Change the Floyd while-condition to `fast->next != nullptr (for odd cases) && fast->next->next != nullptr (for even cases)`.

### Odd LLs
For odd LLs, the extra node will be included in the right half. Since our palindrome while-condition is `leftHead != nullptr && rightHead != nullptr`, the loop will break when `leftHead` reaches `nullptr` since it has one node less than the right LL and so the middle element is never checked. 

## Remember to restore the original LL in the end!
