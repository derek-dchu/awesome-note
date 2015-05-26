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