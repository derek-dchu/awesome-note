# Binary Search

### Sqrt(x)
Implement int sqrt(int x) method which returns the first integer that is smaller than or equals to, if exists, square root of x.

For example: 

sqrt(3) = 1  
sqrt(4) = 2

Solution:

```
def sqrt(integer x)
    integer l := 0
    integer r := x + 1 # add 1 for the case that x = 1
    while r - l > 1
        integer mid = (r - l) / 2 + l # prevent overflow
        
        # we are doing integer multiplication instead of float here
        if mid * mid <= x
            l := mid
        else
            r := mid
    return l
```

**Note:** because the method returns an integer, we don't need to perform float multiplication which is much slower than integer multiplication.

### Search for a value in an m x n matrix.
Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.

Solution:
Treat matrix as an array and perform binary search. Remember that `row = index // n` and `col = index % n`.

### Binary search with duplicates
For a given sorted array (ascending order) and a target number, find the first index of this number in O(log n) time complexity. Array may contain duplicates. If the target number does not exist in the array, return -1.

```
l := 0, r := arr.length - 1
while l < r
    if arr[mid] < target
        l := mid + 1
    else
        """
        if equals, we still shrink r, but keep mid as the upper
        bound, because it may be the first index of target 
        """
        r := mid
        
if arr[l] == target
    return l
else
    return -1
```

### Find Minimum in Rotated Sorted Array
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

```
l := 0, r := arr.length - 1
while l < r
    if arr[mid] < arr[r]
        l := mid + 1
    else
        r := mid
        
return arr[l]
```

### Find Peak Element
There is an integer array which has the following features:
  * The numbers in adjacent positions are different.
  * A[0] < A[1] && A[A.length - 2] > A[A.length - 1].

We define a position P is a peek if A[P] > A[P-1] && A[P] > A[P+1].

Find a peak element in this array. Return the index of the peak.

Example:
[1, 2, 1, 3, 4, 5, 7, 6]

return index 1 (which is number 2)  or 6 (which is number 7)

Note: The array may contains multiple peeks, find any of them.

```
# since arr[0] < arr[1], eliminate arr[0]
l := 1
# since arr[arr.length-2] > arr[arr.length-1], 
# eliminate arr[arr.length-1]
r := arr.length - 2
while l < r:
    mid := (r - l) / 2 + l
    if arr[mid-1] < arr[mid]:
        if arr[mid+1] < arr[mid]:
            return mid
        else:
            l := mid + 1
    else:
        r := mid - 1
return l
```

