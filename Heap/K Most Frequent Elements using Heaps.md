**Invariant:** The set holds exactly the K most frequent elements seen so far, ordered by frequency. The minimum (leftmost) is the least frequent of the top-K.

**Why not a heap?** Updating a frequency requires removing the stale `(freq-1, val)` entry. C++ `priority_queue` doesn't support arbitrary erase. An ordered set supports O(log K) insert and erase by value, so stale entries are explicitly evicted before inserting the updated entry, keeping the window clean.

```cpp
class KthMostFrequent {
    int _k;
    set<pair<int,int>> _window;   // {frequency, value}, ordered by frequency
    unordered_map<int,int> _freq;

public:
    KthMostFrequent(int k) : _k(k) {}

    void add(int val) {
        _window.erase({_freq[val], val});   // no-op if val unseen (erases {0,val} which isn't present)
        _freq[val]++;
        _window.insert({_freq[val], val});

        if ((int)_window.size() > _k)
            _window.erase(_window.begin()); // evict least frequent
    }

    vector<int> getTopK() {
        vector<int> res;
        for (auto& [cnt, val] : _window)
            res.push_back(val);
        return res;
    }
};
```

**Time:** O(log K) per `add` | **Space:** O(K) 

### A Note on using Priority Queue with Stale entries + Lazy Deletion
When frequencies update, a new `(freq, val)` entry is pushed without removing the old `(freq-1, val)` — creating a stale duplicate. Lazy deletion cleans stale entries when they surface to the top, but stale entries buried in the middle are invisible until they reach the top. This means they occupy valid slots and can cause correct elements to be wrongly evicted when the size check fires.

```cpp
void add(int val) {
    freq[val]++;
    minHeap.push({freq[val], val});

    // clear stale entries from top
    while (!minHeap.empty() && minHeap.top().first != freq[minHeap.top().second])
        minHeap.pop();

    if ((int)minHeap.size() > k)
        minHeap.pop();
}
```

K=2, stream = `[a, b, c, c]`:

```
freq after all: a=1, b=1, c=2
correct top-2: c, and either a or b
```

```
add(a): heap={(1,a)},                size=1
add(b): heap={(1,a),(1,b)},          size=2
add(c): heap={(1,a),(1,b),(1,c)},    no stale at top, size=3 > K → pop (1,a)
        heap={(1,b),(1,c)}
add(c): heap={(1,b),(1,c),(2,c)},    no stale at top, size=3 > K → pop (1,b)
        heap={(1,c),(2,c)}
```

`getTopK` returns `{c, c}` — stale `(1,c)` is buried and never cleaned, so both slots are occupied by `c`. `b` was wrongly evicted.