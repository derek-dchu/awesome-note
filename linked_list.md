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