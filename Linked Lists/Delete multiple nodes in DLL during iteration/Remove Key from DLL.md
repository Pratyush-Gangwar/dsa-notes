## Code
```cpp
/* a Node of the doubly linked list
class Node {
  public:
    int data;
    Node* next;
    Node* prev;

    Node(int x) {
        data = x;
        next = nullptr;
        prev = nullptr;
    }
};
*/

class Solution {
  public:
    Node* delNode(Node* node) {
        Node* prev = node->prev;
        Node* next = node->next;

        if (prev != nullptr) prev->next = next;
        if (next != nullptr) next->prev = prev;

        delete node;
        return next;
    }

    // Function to delete a specified node from the linked list
    Node* deleteAllOccurOfX(Node* head, int x) {
        Node* curr = head;

        while (curr != nullptr) {

            if (curr->data == x) {
                bool isHead = false;
                if (curr->prev == nullptr) isHead = true;

                curr = delNode(curr);
                if (isHead) head = curr;
            }

            else {
                curr = curr->next;
            }
        }

        return head;
        
    }
};
```