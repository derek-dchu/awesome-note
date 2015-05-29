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
Consider each word as a vertex in a undirect graph, we add edge between two words if and only if they have only one mismatching letter. Then the problem reduce to finding a shortest path from start vertex to end vertex with each edge has weight 1. In this cases, BFS can serve the purpose.

For example, dictionary ["hit","hot","dot","dog"] has following graph.

```
hit  dot
 |  / |
 | /  |
hot  dog
```

By doing BFS, we can easily find that there is only one path from "hit" to "dog".

**Important Notes:** 
Constructing the graph is time consuming, because we may need to perform $$n^2$$ comparisons to find all edges. One trick to reduce number of comparisons is we put words into buckets first, then we add edges between words within a bucket.

For each word, we consider it fitting k buckets where k is the length of word. For example, "hit" fits "?it", "h?t", "hi?" buckets, "hot" fits "?ot", "h?t", "hi?". We have "hit" and "hot" both fit in bucket "h?t", then we know that they meet the criteria and add an edge between them.
    
In this way, we limit comparison between related words, because in real problems (words in English dictionary), words have only one mismatching letter are much less than random pairs.

##### Pseudocode
###### BFS
```python
def ladderLength(self, start, end, dictionary):
    if start is None or end is None or dictionary is None:
        return 0
    # generate buckets
    buckets = {}
    for word in dictionary:
        for i in range(len(word)):
            bucket = word[:i] + '?' + word[i+1:]
            if bucket in buckets:
                buckets[bucket].append(word)
            else:
                buckets[bucket] = [word]
    # add edges to adjacency list
    vertices = {word: [] for word in dictionary}
    for b in buckets.keys():
        for i in range(len(buckets[b])):
            for j in range(i+1, len(buckets[b])):
                vertices[buckets[b][i]].append(buckets[b][j])
                vertices[buckets[b][j]].append(buckets[b][i])
    # BFS
    vertex_visited = {word: False for word in dictionary}
    vertex_distance = {start: 1}
    from collections import deque
    q = deque()
    q.append(start)
    vertex_visited[start] = True
    while len(q) > 0:
        curr_vert = q.popleft()
        for nbr in vertices[curr_vert]:
            if nbr == end:
                return vertex_distance[curr_vert] + 1
            if not vertex_visited[nbr]:
                vertex_distance[nbr] = vertex_distance[curr_vert] + 1
                q.append(nbr)
                vertex_visited[nbr] = True
    return 0
```

### N-Queens
The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' indicates a queen, and '.' indicates an empty space respectively.

##### Analysis
Use 8-queens puzzle as an example, where n = 8.

*  Brute Force:  

    Check all $$8^8$$ possibilities, and output thoses valid configurations.

*  Permutation:  

    One basic strategy is that, we place queens in different rows one by one, and for each queen, it cannot be placed in a column that exists a queen. In other words, possibilites reduces to $$8!$$. We can also guarantee that there is no row and col violation.

    We can use ***backtracking depth-first search*** to examine all possibilites and optimize with constraint (backtraking tree early prune).
    
    A useful constraint is that for each newly places queen, it cannot violate diagonal constraint with previously placed queens.

##### Pseudocode
###### Backtracking
```
define solveNQueens(n)
    avaliable_cols := {1 .. n}
    used_cols := {}
    locations = {}
    solve(n, avaliable_cols, used_cols, locations)
    return configurations
    
define solve(n, avaliable_cols, used_cols, result)
    # if we have placed n queens, it is a valid configuration
    if avaliable_cols.length == 0 then
        n = len(used_cols)-1
            result.append(["."*col+"Q"+"."*(n-col) for col in used_cols])
        else:
            for col in list(avaliable_cols):
                avaliable_cols.remove(col)
                if self.is_valid(used_cols, col, n):
                    used_cols.append(col)
                    self.solve(n, avaliable_cols, used_cols, result)
                    used_cols.pop()
                avaliable_cols.add(col)

    def is_valid(self, used_cols, col, n):
        r = len(used_cols) - 1
        left_col = col-1
        right_col = col+1
        while r >= 0 and (left_col >= 0 or right_col < n):
            if used_cols[r] == left_col or used_cols[r] == right_col:
                return False
            r -= 1
            left_col -= 1
            right_col += 1
        return True
```