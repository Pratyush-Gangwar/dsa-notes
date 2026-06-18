	## Setup
- **$n_{entry}$** = zero-indexed position of the cycle head
- **C** = length of the cycle
- Slow moves **1 step/iteration**, fast moves **2 steps/iteration**

## A word of caution
Everything below is measured in terms of *indices*. The typical formula $l - a + 1$ for calculating the number of items between two numbers (ends inclusive) does not apply to calculating *relative indices*. 

For example, consider a cycle whose head is at global index 1. Then, the indexing inside the cycle from its head is calculated as $\text{node global index} - \text{head global index}$. There is no +1 because we are not *counting* items but rather calculating *relative indices*.

If your current index in the linked list is $i$ and you move one node per iteration, then,
- after 1 iteration, it will be $i + 1$
- after 2 iterations, it will be $i + 2$
- ..., after $x$ iterations, it will be $i + x$

## When and where do they first meet?
After $t$ iterations,
- Slow is at position **t** (from head, zero-indexed)
- Fast is at position **2t** (from head, zero-indexed)

Once both are inside the cycle, their positions _within the cycle_ are what matter. Slow enters the cycle at $t = n_{entry}$. So, after $t \geq n_{entry}$,
- Slow's position in cycle (zero-indexed): $(t − n_{entry}) \pmod C$
- Fast's position in cycle (zero-indexed): $(2t − n_{entry}) \pmod C$

Note, the cycle position index and the global position index are not equal! Example, position 1 globally means node with value 2. Whereas position 1 in the cycle means node with value 0. 
![[Pasted image 20260614160835.png]]

They meet when these are equal:
$$(t − n_{entry}) = (2t − n_{entry}) \pmod C \implies t = 0 \pmod C \implies t = kC$$

We want $t = kC$ such that $t \geq n_{entry}$. So, $kC \geq n_{entry} \implies k \geq \lceil C/n_{entry} \rceil$ (since $k$ can only be an integer). The **first** meeting happens when $t = \lceil C/n_{entry} \rceil C$.

Plugging this value into slow's position in the cycle, we get the point of meeting as $kC - n_{entry} \pmod C$ in cycle coordinates. We can reduce this to $C - n_{entry} \pmod C$. Note, we could've reduced it to $-n_{entry} \pmod C$ as well but that's the same thing. 
