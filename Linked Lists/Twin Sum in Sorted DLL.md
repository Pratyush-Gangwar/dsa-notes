## Code
```cpp
// User function Template for C++

/* Doubly linked list node class
class Node
{
public:
    int data;
    Node *next, *prev;
    Node(int val) : data(val), next(NULL), prev(NULL)
    {
    }
};
*/

class Solution {
  public:
    Node* getEnd(Node* head) {
        while (head->next != nullptr) head = head->next;
        return head;
    }

    vector<pair<int, int>> findPairsWithGivenSum(Node *head, int target) {
        Node* end = getEnd(head);
        vector<pair<int, int>> ans;

        while (head != end && head->prev != end) {
            int sum = head->data + end->data;

            if (sum == target) {
                ans.push_back( {head->data, end->data} );
                head = head->next;
                end = end->prev;
            }

            else if (sum < target) head = head->next;
            else end = end->prev;
        }

        return ans;
    }
};
```

## Important Points
Notice that the while condition is `head != end && head->prev != end`
