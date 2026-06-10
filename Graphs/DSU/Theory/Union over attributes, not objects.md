## The pattern
You have a set of objects. Each object has multiple attributes. Two objects should be in the same component if they share at least one attribute.

The instinct is to make objects the DSU nodes and union pairs that share an attribute. This works but forces you to find those pairs first — O(n²).

The shift is realizing the objects are actually just _describing relationships between their attributes_, and those attributes are the real graph.

## The general template
```
for each object:
    pick any one attribute as the "anchor"
    for each other attribute on this object:
        union(anchor, other_attribute)
```

## Use Dynamic DSU
See `Dynamic DSU`

## Be careful when using `size[x]` (where `x` is the parent of a connected component)
The size will represent the number of attributes in that connected component. It will not represent the number of objects under that component. 

You can often derive a formula that uses the total number of objects (known) and the number of connected components (known) instead of the number of objects in each component.  

## Why you can't do BFS/DFS
Edges are dynamically discovered. 