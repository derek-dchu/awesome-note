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
        dictionary.add(start)
        dictionary.add(end)
        buckets = {}
        for word in dictionary:
            for i in range(len(word)):
                bucket = word[:i] + '_' + word[i+1:]
                if bucket in buckets:
                    buckets[bucket].append(word)
                else:
                    buckets[bucket] = [word]
        
        vertices = {word: set() for word in dictionary}
        for bucket in buckets.keys():
            for i in range(len(buckets[bucket])):
                for j in range(i+1, len(buckets[bucket])):
                    vertices[buckets[bucket][i]].add(buckets[bucket][j])
                    vertices[buckets[bucket][j]].add(buckets[bucket][i])
        # BFS
        vertex_visited = {start: True}
        vertex_distance = {start: 1}
        from collections import deque
        q = deque()
        q.append(start)
        while len(q) > 0:
            curr_vert = q.popleft()
            for nbr in vertices[curr_vert]:
                if nbr == end:
                    return vertex_distance[curr_vert] + 1
                if nbr not in vertex_visited:
                    vertex_visited[nbr] = False
                if not vertex_visited[nbr]:
                    vertex_distance[nbr] = vertex_distance[curr_vert] + 1
                    q.append(nbr)
                    vertex_visited[nbr] = True
        return 0
```

#### Follow Up
Multiple transformation sequence could exist. Print out all those transformation sequences.

For example  
Given:  
start = "hit"  
end = "cog"  
dict = ["hot","dot","dog","lot","log"]

We have  
"hit" -> "hot" -> "dot" -> "dog" -> "cog"  
"hit" -> "hot" -> "lot" -> "log" -> "cog"

##### Analysis
Because we need to print out the path, in addition to depth, we also need to save the parent of current vertex. 

Be careful:
  1. it could be multiple parents, for example:

    ``` 
    a -- b  start: a end: b
    |    |  we will have d's parents as b,c
    c -- d
    ```
    
    Therefore, we have to mark each vertex with 3 states instead of 2 as usual. In above example, after we visit b and explore d, d will be mark as visited, then c will not explore d again, however c is a potential parent of d. By marking d as under_explore (the third state) instead until we visit it (pop from queue), we can solve this problem.
    
  2. Some parents are not valid (which is at the same level of current vertex), for example:
  
    ```
    a --- b     Both a,b will be parent of c.
      \    |    However, b,c are in the same level,
       \__ c    b cannot be a parent of c.
    ```
  3.  We cannot terminate BFS at the first time we meet end vertex, instead, we want to terminate after we exceed the shortest depth.

##### Code
```python
def findLadders(self, start, end, dictionary):
    if start is None or end is None or dictionary is None:
        return 0
    dictionary.add(start)
    dictionary.add(end)
    
    # Same code for building the tree as above
    buckets = {}
    for word in dictionary:
        for i in range(len(word)):
            bucket = word[:i] + '_' + word[i+1:]
            if bucket in buckets:
                buckets[bucket].append(word)
            else:
                buckets[bucket] = [word]
    
    vertices = {word: set() for word in dictionary}
    for bucket in buckets.keys():
        for i in range(len(buckets[bucket])):
            for j in range(i+1, len(buckets[bucket])):
                vertices[buckets[bucket][i]].add(buckets[bucket][j])
                vertices[buckets[bucket][j]].add(buckets[bucket][i])
                
    # BFS
    # three states: white, black, gray
    vertex_visited = {start: "white"}
    
    # record parent of each vertex
    vertex_parent = {start: set()}
    vertex_distance = {start: 1}
    shortest_distance = None
    from collections import deque
    q = deque()
    q.append(start)
    while len(q) > 0:
        curr_vert = q.popleft()
        vertex_visited[curr_vert] = "black"
        
        # if current depth exceed shortest_distance, terminate BFS
        if vertex_distance[curr_vert] == shortest_distance:
            break
        for nbr in vertices[curr_vert]:
            if nbr not in vertex_visited:
                vertex_visited[nbr] = "white"
            if vertex_visited[nbr] != "black":
                if vertex_visited[nbr] == "white":
                    vertex_distance[nbr] = vertex_distance[curr_vert] + 1
                    q.append(nbr)
                    vertex_visited[nbr] = "gray"
                if nbr not in vertex_parent:
                    vertex_parent[nbr] = set()
                if vertex_distance[curr_vert] != vertex_distance[nbr]:
                    vertex_parent[nbr].add(curr_vert)
            
            # instead of terminating BFS, we record the shortest distance
            if nbr == end and shortest_distance is None:
                shortest_length = vertex_distance[nbr]
                break
    
    # build all paths
    result = []
    self.build_path(vertex_parent, end, [], result)
    return result
    
# build all paths using backtracking
def build_path(self, vertex_parent, end, path, result):
    if end not in vertex_parent:
        return
    if  len(vertex_parent[end]) == 0:
        temp = path[:]+[end]
        temp.reverse()
        result.append(temp)
    path.append(end)
    for v in vertex_parent[end]:
        self.build_path(vertex_parent, v, path, result)
    path.pop()
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
    configurations := {}
    solve(n, avaliable_cols, used_cols, configurations)
    return configurations
    
define solve(n, avaliable_cols, used_cols, configurations)
    # if we successfully placed n queens, it is a valid configuration
    if used_cols.length == n then
        generate configuration based on the
        order of used_cols (row by row) and
        append it to configurations
    else
        for col in avaliable_cols
            remove col from avaliable_cols
            # if it is valid after placed a queen in this col
            if is_valid(used_cols, col, n) then
                append col to used_cols
                # place next queen
                solve(n, avaliable_cols, used_cols, result)
                # backtrack to previous state
                pop col from used_cols
            # backtrack to previous state
            add col back to avaliable_cols

define isValid(used_cols, col, n)
        check whether placing a queen at col violates
        diagonal constraint with previous queen at used_col
```

