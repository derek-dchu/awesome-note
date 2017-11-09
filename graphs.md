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
# make sure explore every vertices, because we may start at a sink vertex which has not out edges
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

## Directed Acyclic Graphs \(DAG\)

#### Topological Sort

```
TopologicalSort(G):
    DFS(G)
    sort vertices by reverse post-order
```

## Strongly Connected Components \(SCC\)

* Vertices in SCC are connected to each other
* Can partition vertices into SCC
* Metagraph describes how SCC connect to each other
* Metagraph always a DAG

#### Finding SCCs

```
SCCs(G):
    DFS(reversed-G) // reversed G is a graph of G that all edges are reversed
    for v in V of G in reverse postorder:
        if not visited(v):
            explore(v)
            mark visited vertices as new SCC
```

DFS on both reversed-G and G =&gt; Runtime O\(\|V\| + \|E\|\)



