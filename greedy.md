# Greedy

### Gas Station
There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

For a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). The journey starts with an empty tank at one of the gas stations.

Return the starting gas station's index if it can travel around the circular route once, otherwise return -1.

Example
Given 4 gas stations with gas[i]=[1,1,3,1], and the cost[i]=[2,2,1,1]. The starting gas station's index is 2.

Note  
The solution is guaranteed to be unique.

Challenge  
O(n) time and O(1) extra space

##### Analysis
*  Suppose we start from $$i$$, and keep counting summation $$sum$$ =+ $$gas[j] - cost[j]$$, when sum becomes negative at j, we know that the journey cannot continue => i cannot be starting point.
*  What if we start at one of the station from $$i+1$$ to $$j$$ ? Because we already knew that $$i$$ can go to $$k$$, so $$i + k$$ cannot go to $$j$$.
*  In other words, when we have a $$sum$$ is negative, reset our starting point to $$j+1$$, and reset $$sum$$ to 0 in order to remove previous records.
*  Eventually, we will obtain an optimum starting point. We can prove that if the total gas >= total cost, there must be a solution, which is the optimum starting point.

##### Pseudocode
```
sum := 0
total := 0
start_index := 0
for j in 1 .. n
    diff := gas[j] - cost[j]
    sum += diff
    total += diff
    if sum < 0 then
        start_index := j + 1
        sum := 0
if total < 0 then
    return -1
return start_index
```

### Largest Number
Given a list of non negative integers, arrange them such that they form the largest number. The number can be very big, so return it in string.

Example  
Given [1, 20, 23, 4, 8], the largest formed number is 8423201.

##### Analysis
From intuitive, if we convert integers to string, we find following principals to arrange them:
  1. Find the first mismatch digit, always put the bigger one on left to the other one. 
    ```
    9XX, 4XX, 8XX => 9XX8XX4XX
    ```
  2. For a digit pattern A, if A is a subpattern, from the first digit, of the other digit pattern B, check for the first extra digit, if it is smaller than the first digit of A, put B on right. Otherwise, put A on left.
    ```
    82, 82900, 82199 => 82900|82|82199
    ```
    
All in all, we perform a string sorting following above principals, and then concatenate them to create the largest number.

Complexity: O(nlogn), Space: O(n)
    
##### Pseudocode
```
num_string = {convert n to string for n in arr}
sort num_string based on following compare method
# numbers could be all zeros => {"0", "0"} => "00" => "0"
if num_string[0] == "0"
    return "0"
return concatenate of all str in num_string

define cmp(a, b)
    # return -1: put a on the left of b
    # return 0: order of a and b doesn't match
    # return 1: put a on the right of b
    for i in 1 .. min(a.length, b.length)
        if a[i] > b[i]
            return -1
        else if a[i] < b[i]
            return 1
    if a.length > b.length
        if a[b.length] < b[0]
            return 1
        else
            return -1
    if a.length < b.length
        if b[a.length] < a[0]
            return -1
        else
            return 1
    return 0
```