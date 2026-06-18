## Code
```cpp
/* Structure of a link list node
class Node {
  public:
    int data;
    Node* next;
    Node* prev;
    Node(int value) {
        data = value;
        next = nullptr;
        prev = nullptr;
    }
};
*/
class Solution {
  public:
    Node* delNode(Node* node) {
        Node* next = node->next;
        Node* prev = node->prev;
        
        if (next) next->prev = prev;
        if (prev) prev->next = next;
        
        delete node;
        return next;
    }
    
    Node* removeDuplicates(Node* headRef) {
        if (!headRef) return nullptr;
        Node* curr = headRef->next;
        
        while (curr) {
            if (curr->prev->data == curr->data) {
                bool isHead = (curr == headRef);
                
                curr = delNode(curr);
                if (isHead) headRef = curr;
            }
            
            else {
                curr = curr->next;
            }
        }
        
        return headRef;
    }
};
```
