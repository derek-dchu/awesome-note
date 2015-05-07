# Array

### Find the two repeating elements in a given array
Given an array of n+2 elements with all elements of the array are in range 1 to n, and all elements occur once except two numbers which occur twice. Find the two repeating numbers.

For example, array = {4, 2, 4, 5, 2, 3, 1} and n = 5

The above array has n + 2 = 7 elements with all elements occurring once except 2 and 4 which occur twice. So the output should be 4 2.

Solution:  
1. Double Loop:O($$n^2$$), Space: O(1)
2. Count Array: O(n), Space: O(n)
3. Base on math: O(n), Space: O(1)  
    * X+Y = sum(arr) - n(n+1)/2  
    * XY = mul(arr) / n!  
4. **Use array element as index**: O(n), O(1)  
    The idea is that, since all elements are in range 1-n, we can mark the ith element to negative if we meet an integer equals to i. If i is a repetition, then we will revisit the ith element which has been marked to negative.

    Pseudocode
    ```
    for i := 1..n
        index := abs(arr[i])
        if arr[index] > 0
            // mark it negative
            arr[index] = -Arr[index]
        else
            // arr[index] is negative, we revisit integer i
            integer i is a repetition
    ```
    
### Find duplicates in a given array
Given an array of n elements which contains elements from 0 to n-1, with **ANY** of these numbers appearing **ANY** number of times. Find these repeating numbers in O(n) and using only constant memory space.

### Find the smallest positive integer missing from an unsorted array
Given an unsorted array with both **positive** and **negative** integers. Find the smallest positive integer missing from the array in O(n) time using constant extra space. The array can be modified.

Solution:
1. Double Loop: O($$n^2$$), Space: O(1)
2. Sorting: O(nlogn), Space: O(1)
3. Hashing: O(n), Space: O(1)
4. **Use array element as index**: O(n), Space: O(1)  
    Negative integers cause a problem here. We can first segregate them to the right side.

    Pesudocode  
    ```
    len := length of arr
    // segregate
    i := -1
    j := len
    while i < j
        i := i + 1
        j := j - 1
        while arr[i] > 0
            i := i + 1
            if i >= len
                break
        while arr[j] <= 0
            j := j - 1
            if j <= 0
                return 1
        if i < j
            swap arr[i] with arr[j]
    len := j + 1
    
    // mark integer using array element as index
    for i in 0..len-1
        index = abs(arr[i]) - 1
        if index < len and arr[index] > 0
            arr[index] = -arr[index]
            
    // look for missing positive integer
    for i in 0..len-1
        if arr[i] > 0
            return i + 1
    return len + 1
    ```

### 2 Sum, 3 Sum [closest], 4 Sum
#### 2 Sum
Given an array of integers, find two numbers such that they add up to a specific target number. Result should indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note indices are **NOT** zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

#### 3 Sum

Solution:
1. Brute Force:
  * Two Sum: O($$n^2$$)
  * 3 Sum: O($$n^3$$)
  * 4 Sum: O($$n^4$$)
2. Sorting:
  We first sort the array, and then use both left pointer and right pointer to search for the remaining integer(s) by shrinking both pointers => O(n). We can just check the entries from i + 1 to array.length – 1 because of the order of indices.
  * Two Sum: O(nlogn) + O(n) = O(nlogn)
  * 3 Sum [closest]: O(nlogn) + O($$n^2$$) = O($$n^2$$)
  
    Pesudocode
    ```
    sort(arr)
    # if all elements are not less than target
    if arr[0] >= target
        return arr[0] + arr[1] + arr[2]
        
    len := arr.length - 2
    closest_distance = max of int
    closest_sum = null
    for i in 0..len
        remain := target - number[i]
        l := i + 1
        r := len - 1
        while l < r
            sum_of_two := arr[l] + arr[r]
            if sum_of_two == remain
                return target
                
            sum_of_three = sum_of_two + arr[i]
            distance = abs(sum_of_three - target)
            if distance == closest_distance
                closest_distance = distance
                closest_sum = sum_of_three
            if sum_of_two < remain
                l := l + 1
            else
                r := r - 1
    ```
    
  * 4 Sum: O(nlogn) + O($$n^3$$) = O($$n^3$$)
3. HashMap:
  We can store integers into a hashmap with their index as value to reduce lookup time from O(n) to O(1).
  * Two Sum: O(1), Space: O(n)
  * 3 Sum: O($$n^2$$), Space: O(n)
  * 4 Sum: O($$n^3$$), Space: O(n)



