## Naive Approach
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> freqs;
        int left = 0;
        int maxLen = 0;

        for(int right = 0; right < s.size(); right++) {
            char ch = s[right];
            freqs[ch]++;
            
            while (freqs[ch] > 1) {
                freqs[ s[left] ]--;
                left++;
            }

            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

## Replacing `map` with `vector`
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        vector<int> freqs(256, 0);
        
        int left = 0;
        int maxLen = 0;

        for(int right = 0; right < s.size(); right++) {
            char ch = s[right];
            freqs[ch]++;
            
            while (freqs[ch] > 1) {
                freqs[ s[left] ]--;
                left++;
            }

            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

## Optimizing away the while-loop
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        vector<int> lastSeen(256, -1); // IMP

        int left = 0;
        int maxLen = 0;

        for(int right = 0; right < s.size(); right++) {
            char ch = s[right]; 
            
            if ( lastSeen[ch] >= left ) { // IMP
                left = lastSeen[ch] + 1; // IMP
            }

            lastSeen[ch] = right; // IMP
            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

The `lastSeen[ch] >= left` check is important. It is possible that `lastSeen[ch] != -1` (that is the character was seen before) but `< left`. In that case, if we set `left = lastSeen[ch] + 1`, we'd actually be EXPANDING the window (by going to the past).