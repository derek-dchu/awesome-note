# Binary Tree

### Binary Tree Preorder Traversal
Given a binary tree, return the preorder traversal of its nodes' values.

Example  
Given binary tree {1,#,2,3}:

```
1
 \
  2
 /
3
```
return [1,2,3].

##### Analysis
*  Recursive:
    ```
    add this.val into array
    preorderTraversal(this.left)
    preorderTraversal(this.right)
    ```

*  Iterative  
Use stack instead

##### Pseudocode
*  Recursive
    ```
    define preorderTraversal(root, arr)
        if root is not null then
            arr append root.val
            preorderTraversal(root.left)
            preorderTraversal(root.right)
        return
    ```

*  Iterative
    ```
    stack := {root}
    arr := []
    while stack is not empty
        node := stack.pop()
        arr append node.val
        if node.right is not null then
            stack.push(node.right)
        if node.left is not null then
            stack.push(node.left)
    return arr
    ```

### Valid Height-Balanced Binary Tree
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example  
Given binary tree A={3,9,20,#,#,15,7}, B={3,#,20,15,7}

```
A)  3            B)    3 
   / \                  \
  9  20                 20
    /  \                / \
   15   7              15  7
```

The binary tree A is a height-balanced binary tree, but B is not.

##### Analysis
By modified the algorithm for finding depth of subtrees, we can check the difference between depths of two subtrees. If depths are differ by more than 1, we can terminate process by return -1. Otherwise, we return the larger height plus one as height of current subtree. In addition, if we have either left height or right height equals to -1, which means either left subtree or right subtree is not balanced, then current subtree is also not balanced.

##### Pseudocode
```
define helper(root)
if root is null then
    return 0
left_height = helper(root.left)
if left_height == -1 then
    return -1
right_height = helper(root.right)
if right_height == -1 or abs(left_height - right_height) > 1 then
    return -1
return max(left_height, right_height) + 1
```

### Binary Tree Level Order Traversal
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level). The implement should use only one queue.

Example
Given binary tree {3,9,20,#,#,15,7},
```
    3
   / \
  9  20
    /  \
   15   7
```

return its level order traversal as:
```
[
  [3],
  [9,20],
  [15,7]
]
```

##### Analysis
We know that level order traversal is equivalent to breadth first search (BFS). The problem is how do we know when a level is end. We can simply have two counters, one for current level of nodes and another one for next level of nodes. When current level counter reduce to 0, we know that current level is end, then we assign next level counter as current level counter, and reset next level counter.

##### Pseudocode
```
arr := {}
q := new queue
put root into q
curr_level := 1
next_level := 0
level := {}
while size of q != 0
    node := pop from q
    curr_level := curr_level - 1
    level append node.val
    if node.left is not null then
        put node.left into q
        next_level := next_level + 1
    if node.right is not null then
        put node.right into q
        next_level := next_level + 1
    if curr_level == 0 then
        arr append level
        level = {}
        curr_level := next_level
        next_level := 0
return arr
```


