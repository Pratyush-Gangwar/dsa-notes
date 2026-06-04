## Clarifying the problem
At most k distinct does not mean the string length has to be at most k. Rather, it just means that you can have at most k unique characters (each of which may have duplicates).

## while-loop
```cpp
int maxLen = 0;
unordered_map<char, int> freqs;
int left = 0; 

for(int right = 0; right < str.size(); right++) {
    freqs[str[right]]++;
    
    while (freqs.size() > k) {
        freqs[str[left]]--;
        if (freqs[str[left]] == 0) freqs.erase(str[left]);
        left++;
    }
    
    maxLen = max(maxLen, right - left + 1);
}

return maxLen;
```

Note, you must use a map and not a set. 

## Replacing while with if
```cpp
int maxLen = 0;
unordered_map<char, int> freqs;
int left = 0;

for (int right = 0; right < str.size(); right++) {
    freqs[str[right]]++;

    // shrink the window by one if it becomes invalid
    if (freqs.size() > k) {
        freqs[str[left]]--;
        if (freqs[str[left]] == 0) freqs.erase(str[left]);
        left++;
    }

    // update maximum length only if window is valid
    if (freqs.size() <= k) {
        maxLen = max(maxLen, right - left + 1);
    }
}

return maxLen;
```

## Wrong optimization

Inspired by the “longest substring with no duplicates” idea, you might think to jump left past the last occurrence of s[left] = 'a'

But this is wrong as this example shows:
`s = "abacccc", K = 2`
- First invalid window: "abac" → 3 distinct characters (a, b, c)
- If you jump left to the last occurrence of `s[left] = 'a'` + 1 (index 2 → left = 3), and report "cccc" as the longest.
- But substring "acccc" is actually the longest.

✅ Takeaway: Jumping by last occurrence assumes the leftmost character must be removed, but in this problem you might be able to remove a character in the middle to get a bigger window (in this case, if we remove `b`, we can keep the last `a`) — so shrinking must be done step by step.