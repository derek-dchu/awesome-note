# Search & Recursion

### Topological Sorting
Given a digraph, put the vertices in order such that all its directed edges point from a vertex earlier in the order to a vertex later in the order (or report that doing so is not possible).

##### Analysis
*  DFS: A reverse DFS postorder in a DAG provides a topological order.
*  BFS:

##### Pseudocode
Suppose each node contains a label for 1 to n, n is the number of nodes
*  DFS

    ```
    def topo_sort(graph)
        sorted_list = {}
        marked = {False .. False} # length = n
        # reverse DFS postorder
        for each node in graph
            dfs_postorder(node, marked, topo_list)

        reverse order of topo_list
        return topo_list
        
    def dfs_postorder(node, marked, topo_list)
        if marked[node.label] is true then
            return
        set marked[node.label] to true
        for each node as n in node's neighbors list
            if marked[n.label] is false then
                dfs_postorder(n, marked, topo_list)
        # postorder, append after recursion
        append node to topo_list
    ```
    
### Word Ladder
Given two words (start and end), and a dictionary, find the length of shortest transformation sequence from start to end, such that:

Only one letter can be changed at a time
Each intermediate word must exist in the dictionary

Example  
Given:  
start = "hit"  
end = "cog"  
dict = ["hot","dot","dog","lot","log"]  
As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog", return its length 5.

Note  
*  Return 0 if there is no such transformation sequence.
*  All words have the same length.
*  All words contain only lowercase alphabetic characters.

##### Analysis
###### BFS
Consider each word as a vertex in a undirect graph, we add edge between two words if and only if they have only one miss matching letter. Then the problem reduce to finding a shortest path from start vertex to end vertex with each edge has weight 1. In this cases, BFS can serve the purpose.

For example, dictionary ["hit","hot","dot","dog"] has following graph.

```
hit  dot
 |  / |
 | /  |
hot  dog
```

By doing BFS, we can easily find that there is only one path from "hit" to "dog".

**Important Notes:** 
1.  Constructing the graph is time consuming, because we may need to perform $$n^2$$ comparisons to find all edges. One trick to reduce number of comparisons is we put words into buckets first.

    For each word, we consider it fitting k buckets where k is the length of word. For example, "hit" fits "?it", "h?t", "hi?" buckets, "hot" fits "?ot", "h?t", "hi?".