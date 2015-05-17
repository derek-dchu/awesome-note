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


### Single Number
Given 2*n + 1 numbers, every numbers occurs twice except one, find it.

Example  
Given [1,2,2,1,3,4,3], return 4

##### Analysis
* 0 ^ anything = 0
* anything ^ itself = 0

#### Follow Up 1
Given 3*n + 1 numbers, every numbers occurs triple times except one, find it.

##### Analysis
If all numbers are 32bits integer, we can count number of 1s for each bit. Because we know, final count for the ith bit must be following:

    3i * 0 + 3j * 1 + 0 or 1, where i + j = n
    
so, for each count, we have that if `count % 3 == 1`, the except number is set at the bit, otherwise, it is unset.

##### Pseudocode
```
bit_count = {0,0, ... ,0} // 32 length
for number as n in arr
    mask = 1
    for i in 32 .. 1
        if i & mask != 0 then
            bit_count[i] := bit_count[i] + 1
        mask := mask << 1
binary_form = ""
for count as c in bit_count
    if c % 3 != 0 then
        add '1' into binary_form
    else
        add '0' into binary_form
return an integer with the binary_form
```


### Majority Number
Given an array of integers, the majority number is the number that occurs **more than half** of the size of the array. Find it.

Example  
Given [1, 1, 1, 1, 2, 2, 2], return 1

##### Analysis
[Linear Time Majority Vote Algorithm](http://www.cs.utexas.edu/~moore/best-ideas/mjrty/index.html): O(n), Space O(1)

##### Pseudocode
```
count := 0
majority_element := null
for element as e in arr
    if count == 0 then
        majority_element = e
        count := 1
    else
        if e == majority_element then
            count := count + 1
        else
            count := count - 1

# majority_element is a potential result.
# we need to make sure it by count its occurrences.
if count of majority_element > arr.length / 2 then
    majority_element is the acutal result.
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

### Delete Digits
Given string A representative a positive integer which has N digits, remove any k digits of the number, the remaining digits are arranged according to the original order to become a new positive integer. Make this new positive integers as small as possible.

Constraint  
N <= 240 and k <= N

Example  
Given an integer A="178542", k=4

return a string "12"

##### Analysis
Look at "178542" each digits, we can easily find that for each round:

  1. remove 8 => 17542
  2. remove 7 => 1542
  3. remove 5 => 142
  4. remove 4 => 12
    
They are all the first digit that is larger than its next digit in their round. Following this rule, we can return the result with O(nk) complexity (O(n) for k rounds).

However, we don't need to start from the beginning for next round. After remove 8, we can backtrack to 7 and start from there. By doing so, we can make sure that all digits before starting point are smaller or equals to starting point.

Be careful that, if digits are in sorted order, eventually we will meet the last digit which doesn't has next digit to compare with and should be removed.
For duplicates, we can safely pass them.

##### Pseudocode
```
i := 0
while k > 0:
    if i == arr.length - 1 then
        remove the ith digit from arr
        k := k - 1
        i := i - 1
    if arr[i] > arr[i+1] then
        remove the ith digit from arr
        k := k - 1
        """
        i could be the first digit 
        which cannot backtrack to previous digit
        example: 5,4,3,2,1
        """
        if i != 0
            i := i - 1
    else
        i := i + 1
return all remaining digits in arr.
```

### Jump Game
Given an array of non-negative integers. Each element in the array represents a maximum jump length at that position. Determine if we are able to reach the last index when starting from the first index.

Example  
A = [2,3,1,1,4], return true.

A = [3,2,1,0,4], return false.

##### Analysis
We can keep calculating the maximum index we can reach by adding jump length at that position to its index (i + arr[i]). Remember that if current index larger than maximum index, it means this position is not reachable, and we cannot jump anymore which means the last index is also not reachable.

##### Pseudocode
```
max_index := 0
for i in 1 .. arr.length
    if i > max_index then
        return False
    
    current_max := i + arr[i]
    if current_max > max_index then
        max_index = current_max
        if max_index >= arr.length - 1
            return True
```

#### Follow Up
Also return the minimum number of steps that we need to jump from the first index to the last index.

##### Analysis
We just need to count the each step we extend the maximum index.

##### Pseudocode
```
max_index := 0
count := 0
for i in 1 .. arr.length
    if i > max_index then
        # cannot reach the last index
        return -1
    
    current_max := i + arr[i]
    if current_max > max_index then
        max_index = current_max
        # add counter here
        count := count + 1
        if max_index >= arr.length - 1
            return count
```

### Next Permutation
Given a list of integers, which denote a permutation. Find the next permutation in ascending order.

Example  
For [1,3,2,3], the next permutation is [1,3,3,2]  
For [4,3,2,1], the next permutation is [1,2,3,4]

Note  
The list may contains duplicate integers.

##### Analysis
Start from the end of list, we are looking the first decreasing point (arr[i] < arr[i+1]). We know that we can generate next permutation by replace the decreasing point next larger number, and then sort rest numbers on its right in acsending order.

For example:  
1. `1|2|543 => 1|3|245`, 3 is next avaliable number for 2
2. `1|2|432 => 1|3|224`, 3 is next avaliable number for 2
3. `4321`, if we cannot find the decreaing point, that means we reach a last permutation, then we can simply reverse entire list to start over.

##### Pseudocode
```
tail := arr.length
head := tail
while head > 0 then
    head := head - 1
    if arr[head] < arr[head+1] then
        we found the first decreasing point, break loop
# find next available larger number
i = tail
while i > head then
    if arr[i] > arr[head] then
        swap the ith element with head element
        we found next available larger number, break loop
    i := i - 1
if i == head:
    # we cannot find next available larger number.
    reverse the entire list
else:
    reverse rest numbers from head+1 to tail
return arr (It stores next permutation now)
```

