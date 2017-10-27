# Graphs

## Definition

Vertices + Edges

## Representation

|  | Is Edge | List Edge | List Nbrs. |
| :--- | :--- | :--- | :--- |
| Edge List | O\(\|E\|\) | O\(\|E\|\) | O\(\|E\|\) |
| Adjacency Matrix | O\(1\) | O\(\|V\|^2\) | O\(\|V\|\) |
| Adjacency List | O\(deg\) | O\(\|E\|\) | O\(deg\) |

## Exploring

```python
# DFS
Explore(v):
    visited(v) <- True
    
    previsit(v) # optional
    
    for (v, w) in E:
        if not visited(w):
            Explore(w)
            
    postvisit(v) # optional
```

During DFS, we need to keep track of visited vertices to prevent loop. We can also add pre-visit and post-visit orders.

Note that, the range of visited order will not be interleaved.

