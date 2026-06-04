## Count subarrays with sum equal to K (all integers)
```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> prefixCount;

    int prefixSum = 0, count = 0;

    for (int i = 0; i < nums.size(); i++) {
	    // Step 1: Update prefix Sum
        prefixSum += nums[i];
		
		// Step 2: Update answer variable
        if (prefixSum == k)
            count++;
		
		//  2.2 Consider (prefixSum - k) in map case
        if (prefixCount.count(prefixSum - k))
            count += prefixCount[prefixSum - k];
		
		// Step 3: Update map
        prefixCount[prefixSum]++;
    }

    return count;
}
```

## Longest subarray with sum equal to K (all integers)
```cpp
int longestSubarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> firstSeen;

    int prefixSum = 0, maxLen = 0;

    for (int i = 0; i < nums.size(); i++) {
	    // Step 1: Update prefix Sum
        prefixSum += nums[i];
		
		// Step 2: Update answer variable
		// 2.1: Consider prefixSum == k case
        if (prefixSum == k) 
            maxLen = i + 1;
		
		//  2.2 Consider (prefixSum - k) in map case
        if (firstSeen.count(prefixSum - k)) 
            maxLen = max(maxLen, i - firstSeen[prefixSum - k]);
		
		// Step 3: Update map
        if (!firstSeen.count(prefixSum))
            firstSeen[prefixSum] = i;  // only store first occurrence
    }

    return maxLen;
}
```

## Extension to XOR
Since XOR satisfies the prefix property, we can use this method for longest subarray with given XOR or count subarrays with given XOR as well. 