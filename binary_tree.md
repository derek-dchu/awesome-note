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
