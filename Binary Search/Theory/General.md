## Links
https://www.reddit.com/r/leetcode/comments/y8z6ph/binary_search_is_the_bane_of_my_existence/
https://usaco.guide/silver/binary-search?lang=cpp

https://medium.com/@topswebusiness/leetcode-killer-pattern-1-binary-search-61cbacc9f7e7
https://baotramduong.medium.com/leetcode-pattern-19-tips-strategies-for-solving-binary-search-problems-including-10-classic-ed15280a22984
https://towardsdatascience.com/powerful-ultimate-binary-search-template-and-many-leetcode-problems-1f850ef95651
https://leetcode.com/discuss/study-guide/3726061/binary-search-a-comprehensive-guide
https://medium.com/@sanjeev-singh/binary-search-on-answer-5-step-solution-f92a42ebc961

https://www.thealgorists.com/Algo/BinarySearch/AdvancedBinarySearch (Main)
https://leetcode.com/discuss/general-discussion/786126/python-powerful-ultimate-binary-search-template-solved-many-problems (Main)

## Predicate method
https://www.topcoder.com/thrive/articles/Binary%20Search (Main)
https://www.geeksforgeeks.org/binary-search-intuition-and-predicate-functions/
https://cp-algorithms.com/num_methods/binary_search.html#search-on-arbitrary-predicate

STEP 1: Identify search space.
	- This is usually stated in the problem description - indices, elements, etc.
	
STEP 1.5: Identify lower and upper bounds for the search space
	- Lower bound is usually 0
	- Upper bound is ususally last included item + 1 (Exclusive)
	
STEP 2: Identify a monotonic predicate function
STEP 2.5: Prove the validity of the predicate function, i.e, show that if p(x) is true, then p(y) is also true for all y > x.
STEP 3: Identify whether you want first occurence of the true value or last occurence of the false.

STEP 4:	Using predicate function, p(mi) = True implies that p(i > mi) = True and p(mi) = False implies that p( i < mi ) = False.

- Think about which half to discard. 

	- If you're searching for first occurence of truth, then 
		- p(mi) = True => all indices in [mi, hi] satisfy the predicate. 
	
		Since array is sorted, smallest (local) index in this range is mi. So, either mi is the global minimum or there exists some j < mi such that p(j) = True. 
		
		Thus, you discard [mi + 1, hi]. That is your new hi is mi. We are not excluding mi since it is a possible candidate.
		
		(Note that, p(mi) = True doesn't imply that p( j < mi ) = False. It only implies that p( i > mi ) = True.)
	
	
		- p(mi) = False => all indices in [lo, mi] do not satisfy the predicate either.
		
		Thus, you can discard [lo, mi] and your new lo is mi + 1.
		
	
	- If you're searching for last occurence of falsity, then 
		- p(mi) = True => all indices in [mi, hi] satisfy the predicate. 
		
		Thus, you discard [mi, hi]. That is your new hi is mi - 1.
	
		- p(mi) = False => all indices in [lo, mi] do not satisfy the predicate either.
		
		Since array is sorted, largest (local) index in this range is mi. So, either mi is the global maximum or there exists some j > mi such that p(j) = False. 
		
		Thus, you discard [lo, mi - 1]. That is your new lo is mi. We are not excluding mi since it is a possible candidate.
		
		(Note that, p(mi) = False doesn't imply that p( j > mi ) = False. It only implies that p( i < mi ) = False.)
	
	
STEP 5: Check whether [False, True] or [True, False] case with lo = 0, hi = 1 (only one - depending on your monotonic sequence) enters an infinite loop with your given mi rounding (up/down). Use the rounding that works.


- Predicate method + Template
https://leetcode.com/discuss/general-discussion/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems

At the end of this, `lo` points to the desired item (first/last true/false) if the sequence truly was monotonic. But if the question says that the answer may not exist, you might have to check is `lo` is valid and return `-1` accordingly.

ANKI HAS ALL THE NOTES YOU NEED.

# Todo:
- Normal Binary Search, Upper Bound, Lower Bound, First and Last occurence via Precidate method
- Doesn't work for rotated arrays

- Peak element: https://stackoverflow.com/questions/27347747/peak-element-in-an-array?rq=3
- Minimum element in rotated array: 

- Ankify conditions for searching in rotated sorted array

# To Check
https://zhu45.org/posts/2018/Jan/12/how-to-write-binary-search-correctly/
https://stackoverflow.com/questions/44231413/binary-search-using-start-end-vs-using-start-end?rq=1
https://www.reddit.com/r/leetcode/comments/y0r9n6/binary_search_while_left_right_vs_while_left_right/
https://stackoverflow.com/questions/30928268/binary-search-condition/30928332#30928332
https://leetcode.com/discuss/general-discussion/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems
https://www.youtube.com/watch?v=GU7DpgHINWQ
