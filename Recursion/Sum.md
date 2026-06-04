idx, target are the recursion variables

If you can take the same element multiple times, then keep calling recursion with idx as long as `arr[idx] <= target`.

You will only get duplicate answers if the array contains duplicates. If it has all distinct elements (even if you take one element many times), you will never get duplicate answers. To prevent duplicate answers, sort the array so that the duplicates are next to each other and skip all but the first occurrence of a duplicate.