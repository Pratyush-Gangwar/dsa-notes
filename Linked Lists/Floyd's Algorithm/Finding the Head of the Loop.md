## Technique
Now, if your current index in the loop is $C - n_{entry}$ (that is, the meeting point of the slow and fast pointers), then to reach the head of the cycle (index $C$ which reduces to 0 in the modulo), you need $C - n_{entry} + x = C \implies x = n_{entry}$ iterations. 

Now, how do you measure $n_{entry}$ iterations? If your current global index is $0$ (head of the linked list) and you move one node at a time, then after $n_{entry}$ iterations, you would reach the head of the cycle. 

So, if you keep two pointers like this - one at the head of the LL and one at the meeting point of the slow-fast pointers - and move both one node at a time, they'll meet at the head of the loop!

## Does this work for circular linked lists?
Nope. There's no concept of a head.