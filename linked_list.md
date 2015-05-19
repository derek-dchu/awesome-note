# Linked List

### Convert Sorted List to Binary Search Tree
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

##### Analysis
In order to convert into a height **balanced** BST, we need to keep the each insertion of BST as the medimum element of each subarrays. For example, to convert [1,2,3,4,5,6], we insert each element in following order:

```
    3           1    2     5      4    6
   / \       -> / -> | -> / \  -> | -> |
[1,2][4,5,6]   [2]  []  [4][6]    []   []
```

Therefore, we have two approaches:
1. Build the BST from top to bottom, look for medimum element on each level. we know that each level look up time is O($$n/2$$), and we have $$O(logn)$$ levels, so total complexity is $$O(nlogn)$$.
2. Build the BST from bottom to top with a order of traversing the list. Complexity is $$O(n)$$.


##### Pseudocode 1
```
# start, end represent the start and end pointers of subarray 
define recursive helper function(start, end)
    if start == null or start == end then
        return null
    
    slow := start
    fast := start
    while fast != end or fast.next != end then
        slow := start.next
        fast := start.next.next
    treeNode := create an new tree node with val = slow.val
    # recursively call on subarray
    treeNode.left = helper(start, slow)
    treeNode.right = helper(slow.next, end)
    return treeNode
```

##### Pseudocode 2
```
# we need to count the number of list node first
count := 0
curr = head
while curr != null then
    count := count + 1
    curr := curr.next
return helper(0, n-1)

define recursive helper function(start, end)
    if start > end then
        return null
    mid := (end - start) / 2 + start
    leftChild := helper(start, mid-1)
    parent := create an new tree node with val = head.val
    parent.left := leftChild
    
    ### Traversing the list here ###
    head := head.next
    
    parent.right := helper(mid+1, end)
    return parent
```


### Copy List with Random Pointer
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

##### Analysis
The key point is, we have to wait for the copied list to be generated then we can assign random pointers. In other words, we need to remember the mapping between origin list node and copied list node.

1. hash table => hashtable(origin list node, copied list node):  
    Complexity: O(n), Space: O(n)
2. make in place copies: Complexity: O(n), Space: O(1)  
  1. make copies in between origin nodes.

    ```
    1 -> 2 -> 3 => 1 -> (1) -> 2 -> (2) - > 3 -> (3)
    ```
  2. then we can assign random pointer by using following operation
  
    ```
    curr.next.random        =       curr.random.next
            |                                 |
    curr node's copied node's random        |    
        curr node's random pointer pointed node's copied node
    ```
  3. restore the original and copy nodes.
    ```
    origin -> next = origin -> next -> next
    copy -> next = copy -> next -> next
    ```
  4. make sure the last element of origin list node's next points to `null`.