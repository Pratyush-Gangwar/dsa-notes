## Question
You are given an array of size $N - 1$ with integers from 1 to $N$. Exactly one integer is missing. No integers are repeated. Find it. 

## Unsorted 
Missing is just $N(N+1)/2 - sum(arr)$
Time is $O(N)$
## Sorted
Use binary search to find the missing number in $O(\log N)$ time.
The predicate is that the number at `mi` should be `mi - 1`. 
Just before the missing number, this is true. Including and beyond it, this is false. 

```cpp
int findMissing(vector<int>& arr) {
	int lo = 0;
	int hi = arr.size();
	
	while (lo < hi) {
		int mi = lo + (hi - lo)/2;
		
		if (arr[mi] == mi + 1) {
			lo = mi + 1;
		}
		
		else {
			hi = mi;
		}
	}
	
	return lo;
}
```