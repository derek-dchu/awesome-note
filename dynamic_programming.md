# Dynamic Programming
### Triangle
Given a triangle, find the minimum path sum from top to bottom using only O(n) extra space, where n is the total number of rows in the triangle. For each step, it can only move to adjacent numbers on the row below. 

For example, given the following triangle
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).

##### Analysis
It is similar to running sum, we keep tracking all possible sum by doing

$$cache_{r,i} = MIN(cache_{r-1,i-1}, cache_{r-1,i}) + triangle_{r,i}$$

It seems to need to require $$n^2$$ space, because it caches each node of the triangle. However, it actually only need to deal with previous row and current row, because, previous row already had the information of all rows before. And for current row, we directly update cache in place.

##### Pseudocode
```
rows := number of rows in triangle
cache := {MAX_INTEGER} with length = rows
cache[0] := triangle[0][0]
for r in 2 .. rows:
    # update cache inplace, so we also need to store
    # left parent and right parent of each node
    left_parent := cache[0]
    # for the left most
    cache[0] = left_parent + triangle[r-1][0]
    for i in 1 .. r:
        right_parent = cache[i]
        cache[i] = min(left_parent, right_parent) + triangle[r-1][i]
        left_parent = right_parent
    return the min number in the cache which is the minimum sum.
```


### Maximum Subarray
Finding the contiguous subarray within a one-dimensional array of numbers (containing at least one positive number) which has the largest sum.

Example  
{−2, 1, −3, 4, −1, 2, 1, −5, 4} => {4, -1, 2, 1} = 6

[Kadane's Algorithm](http://www.algorithmist.com/index.php/Kadane's_Algorithm)

```
maxSum := -INFINITY
startIndex := 0
endIndex := 0

currentSum := 0
currentStartIndex := 1
for currentEndIndex in 1 .. n
    currentSum := currentSum + arr[currentEndIndex]
    if currentSum > maxSum then
        maxSum := currentSum
        startIndex := currentStartIndex
        endIndex := currentEndIndex
    
    if currentSum < 0 then
        currentSum := 0
        currentStartIndex := currentEndIndex + 1
        
return (maxSum, startIndex, endIndex)
```

Reference  
[Maximum subarray problem](http://en.wikipedia.org/wiki/Maximum_subarray_problem)


### Maximum Product Subarray
Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array [2,3,-2,4], the contiguous subarray [2,3] has the largest product = 6.

##### Analysis
It is similar to the problem finding maximum sum. However, we could have negative number when it meets an new negative number, the product becomes the maximum. Therefore we need to keep tracking the most negative product or just the minimum product. We also need to take care zeros. 

##### Pseudocode
```
prev_max := arr[1]
prev_min := arr[1]
max_prod := arr[1]
for i in 2 .. arr.length
    temp_max := prev_max * arr[i]
    temp_min := prev_min * arr[i]
    curr_max := max_of_three(prev_max, temp_max, arr[i])
    curr_min := min_of_three(prev_min, temp_min, arr[i])
    max_prod := max(curr_max, max_prod)
    prev_max := curr_max
    prev_min := curr_min
return max_prod
```




### Longest Common Substring (LCS)
Suppose we have a source string with length $$p$$ and a target string with length $$q$$, then we can construct a $$\mathit{LCSuffix}$$ table using following formular.

##### Analysis
$$
\mathit{LCSuff}(S_{1..p}, T_{1..q}) = (S[p] = T[q]) \, ? \, \mathit{LCSuff}(S_{1..p-1}, T_{1..q-1}) + 1 : 0
$$

The Longest Common Substring can be found using following formular.
$$
\mathit{LCSubstr}(S, T) = \max_{1 \leq i \leq m, 1 \leq j \leq n} \mathit{LCSuff}(S_{1..i}, T_{1..j}) \;
$$

##### Pseudocode
```
function LCSubstr(S[1..m], T[1..n])
    LCSuffix := matrix(m, n)
    # Length of a LCSubstr
    max := 0
    # Store all LCSubstrs
    result := {}
    for i := 1..m
        for j := 1..n
            if S[i] == T[j]
                if i == 1 or j == 1
                    LCSuffix[i,j] := 1
                else
                    LCSuffix[i,j] := LCSuffix[i-1,j-1] + 1
                if LCSuffix[i,j] > max
                    max := LCSuffix[i,j]
                    result := {S[i-max+1..i]}
                else if LCSuffix[i,j] == max
                        result := result ∪ {S[i-z+1..i]}
            else 
                LCSuffix[i,j] := 0
    return result
```
