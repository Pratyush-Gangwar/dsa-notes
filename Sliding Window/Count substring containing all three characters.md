## Code
```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int count = 0;
        int left = 0;
        unordered_map<char, int> freqs;

        for(int right = 0; right < s.size(); right++) {
            freqs[ s[right] ]++;

            while ( freqs.size() == 3 ) {
                count += (s.size() - 1) - right + 1;

                freqs[ s[left] ]--;
                if ( freqs[s[left]] == 0 ) freqs.erase( s[left] );

                left++;
            }
        }

        return count;
    }
};
```