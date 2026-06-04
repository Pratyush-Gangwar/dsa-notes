## Sliding window condition
`window_size − max_frequency_in_window ≤ k`

## While-loop
When the window shrinks, the previous most frequent character may leave the window, so we must recompute the maximum frequency.
So, we update maxFreq each time.

```cpp
class Solution {
public:
    bool isValid(int left, int right, int k, string& s,
                 unordered_map<char,int>& freqs) {
        int maxFreq = 0;
        for (auto &p : freqs) {
            maxFreq = max(maxFreq, p.second);
        }

        int len = right - left + 1;
        return (len - maxFreq <= k);
    }

    int characterReplacement(string s, int k) {
        int maxLen = 0;
        unordered_map<char,int> freqs;

        int left = 0;
        for(int right = 0; right < s.size(); right++) {
            freqs[s[right]]++;

            if (!isValid(left, right, k, s, freqs)) {
                freqs[s[left]]--;
                left++;
            }

            maxLen = max(maxLen, right - left + 1);
        } 

        return maxLen;
    }
};
```

## If-condition
Axiom: In a sliding window, you can replace a while with if only if adding one element can violate the condition by at most 1.

```cpp
class Solution {
public:
    bool isValid(int left, int right, int k, string& s, unordered_map<char, int>& freqs) {
        int maxFreq = 0;
        for(char ch = 'A'; ch <= 'Z'; ch++) {
            maxFreq = max(maxFreq, freqs[ch]);
        }

        int len = right - left + 1;
        return (len - maxFreq <= k);
    }

    int characterReplacement(string s, int k) {
        int maxLen = 0;
        unordered_map<char, int> freqs;

        int left = 0;
        for(int right = 0; right < s.size(); right++) {
            freqs[ s[right] ]++;

            if ( !isValid(left, right, k, s, freqs) ) {
                freqs[ s[left] ]--;
                left++;
            }

            maxLen = max(maxLen, right - left + 1);
        } 

        return maxLen;
    }
};
```