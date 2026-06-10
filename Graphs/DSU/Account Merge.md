```cpp
class DSU {
    public:
    unordered_map<string, string> parent;
    unordered_map<string, int> size; 

    void insertIfNotExists(string x) {
        if (parent.count(x)) return;

        parent[x] = x;
        size[x] = 1;
    }

    string find(string x) {
        if (parent[x] == x) return x;
        return parent[x] = find(parent[x]);
    }

    void unionBySize(string x, string y) {
        string p_x = find(x);
        string p_y = find(y);

        if (p_x == p_y) return;

        if (size[p_x] < size[p_y]) {
            parent[p_x] = p_y;
            size[p_y] += size[p_x];
        }

        else {
            parent[p_y] = p_x;
            size[p_x] += size[p_y];
        }
    }
};

class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        DSU dsu;
        unordered_map<string, string> accountMap;

        for(auto& account : accounts) {
            string name = account[0];

            for(int i = 1; i < account.size(); i++) {
                string currEmail = account[i];

                accountMap[currEmail] = name;
                dsu.insertIfNotExists(currEmail);

                if (i > 1) dsu.unionBySize(currEmail, account[i - 1]);
            }
        }

        unordered_map<string, set<string>> grouped;
        for(auto& [elem, parent] : dsu.parent ) {
            grouped[parent].insert(elem);
        }

        vector<vector<string>> ans;
        for(auto& [parent, items] : grouped) {
            ans.push_back(vector<string> ());

            ans.back().push_back( accountMap[parent] );
            ans.back().insert(ans.back().end(), items.begin(), items.end() );

            ans.push_back(temp);
        }

        return ans;
    }
};
```