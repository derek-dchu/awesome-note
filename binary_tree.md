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
By modified the algorithm for finding depth of subtrees, we can check the difference between depths of two subtrees. If depths are differ by more than 1, we can terminate process by return -1. 