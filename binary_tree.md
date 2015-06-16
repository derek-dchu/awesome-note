# Binary Tree

### Binary Tree Depth First Traversal
*  Pre-order:
  1. current node
  2. left subtree
  3. right subtree
*  In-order
  1. left subtree
  2. current node
  3. right subtree
*  Post-order
  1. left subtree
  2. right subtree
  3. current node

##### Pseudocode
*  Recursive:
  1. Pre-order
    ```
    define preorderTraversal(root, arr)
        if root is not null then
            append root.val to arr
            preorderTraversal(root.left, arr)
            preorderTraversal(root.right, arr)
        return
    ```
  2. In-order
    ```
    define preorderTraversal(root, arr)
        if root is not null then
            inorderTraversal(this.left, arr)
            append this.val to arr
            inorderTraversal(this.right, arr)
        return
    ```
  3. Post-order
    ```
    define preorderTraversal(root, arr)
        if root is not null then
            postorderTraversal(this.left, arr)
            postorderTraversal(this.right, arr)
            append this.val to arr
        return
    ```
    
*  Iterative:
Use stack instead
  1. Pre-order
    ```
    arr := {}
    stack := {}
    node := root
    while stack is not empty or node is not nil
        if node is not nil then
            append node.val to arr
            if node.right is not null then
                stack.push(node.right)
            node := node.left
        else
            node := stack.pop()
    return arr
    ```
    
  2. In-order
    ```
    arr := {}
    stack := {}
    curr := root
    while stack is not empty or node is not nil
        if node is not nil then
            stack.push(node)
            node := node.left
        else
            node := stack.pop()
            append node.val to arr
            curr = curr.right
    return arr
    ```


### Validate Binary Search Tree
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:
* The left subtree of a node contains only nodes with keys less than the node's key.
*  The right subtree of a node contains only nodes with keys greater than the node's key.
*  Both the left and right subtrees must also be binary search trees.

##### Analysis
Based on the definition of BFS, we can keep shrinking the range (min, max) where child node has to be fit in, using parent node. We can because with Integer.MIN_VALUE and Integer.MAX_VALUE where root has to be fit in. Note that, a node can contains MIN_VALUE and MAX_VALUE, which cannot contain left subtree or right subtree.

##### Pseudocode
```
define isValidBFS(root):
    return helper(root, Integer.MIN_VALUE, Integer.MAX_VALUE)

define helper(root, min, max):
    if root is null then
        return True
    if root.val < min or root.val > max then
        return False
    if root.val == Integer.MAX_VALUE:
        right := root.right is null
    else
        right := helper(root.right, root.val+1, max)
    if root.val == Integer.MIN_VALUE:
        left := root.left is null
    else
        left := helper(root.left, min, root.val-1)
    return left & right
```


### Validate Height-Balanced Binary Tree
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

#### Follow Up 1
Binary Tree Zigzag Level Order Traversal. For each even level, it traverse in reverse order.

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
  [20,9],
  [15,7]
]
```

##### Analysis
We reverse each even level after we finish the original Level Order Traversal.

#### Follow Up 2
Binary Tree Level Reverse Order Traversal. Traverse from leaf level to root level.

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
  [15,7],
  [9,20],
  [3]
]
```

##### Analysis
We reverse the order of all levels after we finish the original Level Order Traversal.


### Serialization/Deserialization of a Binary Tree
Design an algorithm and write code to serialize and deserialize a binary tree. Writing the tree to a file is called *serialization* and reading back from the file to reconstruct the exact same binary tree is *deserialization*.

Example  
An example of using BFS traversal from serialization:
Binary tree {3,9,20,#,#,15,7}, denote the following structure:

```
  3
 / \
9  20
  /  \
 15   7
```


### Construct Binary Tree from Preorder and Inorder Traversal
Given preorder and inorder traversal of a tree, construct the binary tree. Assume that duplicates do not exist in the tree.

Example  
Given pre-order [7,10,4,3,1,2,8,11] and in-order [4,10,3,1,7,11,8,2], return a tree:

```
     ___7___
    /       \
  10         2
 /  \       /
4    3     8
      \   /
       1 11
```

##### Analysis
The key point is, from pre-order list, we can get the current root node, and then use in-order list to seperate remaining nodes into two subtrees. We also know that, remaining nodes in pre-order list actually is the combination of left subtree and right subtree with no intersection.

for example:  
from pre-order[0], we know 7 is the root, then we seperate two list as following:

*  pre-order => 7 | 10,4,3,1 | 2,8,11 |  
*  in-order => | 4,10,3,1 | 7 | 11,8,2 |

Therefore, by doing this seperation recursively, we can reconstruct the binary tree.

##### Pseudocode
```
define recursive_helper (preorder, inorder, p_l, p_r, i_l, i_r)
    if p_l > p_r then
        return nil
    if p_l == p_r then
        return new tree node with val = preorder[p_l]
    # find root val in inorder list
    i := 0
    while inorder[i_l + i] != preorder[p_l]
        i := i + 1
    node := new tree node with val = preorder[p_l]
    node.left = recursive_helper
            (preorder, inorder, p_l+1, p_l+i, i_l, i_l+i-1)
    node.right = recursive_helper
            (preorder, inorder, p_l+i+1, p_r, i_l+i+1, i_r)
    return node
```


### Binary Search Tree
#### Delete Max / Min
*  Delete Max: go right until finding a node that it has a null right link and then replace the link to that node by its left link

*  Delete Min: go left until finding a node that it has a null left link and then replace the link to that node by its right link

#### Delete node with specific value
Maintain a *current* pointer and its *parent* pointer while travsing the tree until we find the value of node pointed by current pointer equal to the specific value.

Current node can be in only three cases:
1.  No child: delete it directly
2.  One child: replace it self with its child
3.  Two children:
    1.  Set x to point to curr's successor (minimum value of curr's right subtree).
    2.  Set the right link of x (which is supposed to point to the BST containing all the keys larger than curr.key) to current right subtree with minimum value is deleted, the link to the BST containing all the keys that are larger than curr.key after the deletion.
    3.  Set the left link of x (which was null) to curr.left (all the keys that are less than both the deleted key and its successor).
    4.  Replace curr with x
    
    ```
    If we want to delete 20
    
      3     
     / \
    9  20
      /  \
     15   27
         /
        25
    
      3 <-- parent  
     / \
    9  20 <-- curr
      /  \
     15   27
         /
        25
    
    Step i: 
      3 <-- parent      
     / \
    9  20 <-- curr
      /  \
     15   27
         /
        25 <-- x
    
    Step ii:
      3 <-- parent      25 <-- x
     / \                 \
    9  20 <-- curr        27
      /  \               /
     15   27            nil
         /
        25
    
    Step iii:
      3 <-- parent      25 <-- x
     / \                / \
    9  20 <-- curr     15 27
      /  \               /
     15   27            nil
         /
        25
    
    Step iv:
      3 <-- parent
     / \
    9  25 <-- x    20 <-- avaliable for garbage collection
      /--\--------/ |
     15   27 -------|
         /
        nil
    ```

