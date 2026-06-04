## Adjacency Matrix
Don't declare variables with the same name (easy to mix V, the number of nodes, and v, the vertice)

## BFS
Queues have `q.front()`, not `q.top()`.

## Word Ladder 1
### Incorrect First Attempt
```cpp
class Solution {
public:


    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> wordSet(wordList.begin(), wordList.end());

        queue<string> q;
        map<string, bool> visited;

        q.push(beginWord);
        visited[beginWord] = true;

        int levelIdx = 0;

        while (!q.empty()) {
            int levelSize = q.size();

            cout << "here1\n";

            while (levelSize != 0) {
                string word = q.front();
                q.pop();

                cout << "here2\n";
    
                if (word == endWord) {
                    return levelIdx;
                }

                cout << "here3\n";

                for(int i = 0; i < word.size(); i++) {
                    string newWord = word;

                    for(char ch = 'a'; ch <= 'z'; ch++) {
                        newWord[i] = ch;
                    }

                    cout << "here4\n";

                    if (wordSet.find(newWord) != wordSet.end()) { 
                        cout << newWord;
                        q.push(newWord);
                        visited[newWord] = true;
                    }
                }

                levelSize--;
            }

            levelIdx++;

        }

        return 0;
    }
};
```

### Mistakes
1. Wrote the valid-word-checking code outside the newWord formation loop
2. Didn't use a visited set and so infinite loop occurred. 
	- Tried to check existence in a set using `if (set.find(elem))`. `find` returns iterators
	- Better yet, instead of creating a separate visited set, just pop elements from the wordSet. 
3. Started `levelIdx` from 0 instead of 1. 