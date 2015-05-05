# Array

### Find the two repeating elements in a given array
For an array of n+2 elements, all elements of the array are in range 1 to n, and all elements occur once except two numbers which occur twice. Find the two repeating numbers.

For example, array = {4, 2, 4, 5, 2, 3, 1} and n = 5

The above array has n + 2 = 7 elements with all elements occurring once except 2 and 4 which occur twice. So the output should be 4 2.

Solution:  
1. Double Loop:O(n*n), Space: O(1)
2. Count Array: O(n), Space: O(n)
3. Base on math: O(n), Space: O(1)  
    * X+Y = sum(arr) - n(n+1)/2  
    * XY = mul(arr) / n!  
4. **Use array element as index**: O(n), O(1)  
    The idea is that, since all elements are in range 1-n, we can mark the ith element to negative if we meet an integer equals to i. If i is a repetition, then we will revisit the ith element which has been marked to negative.

    Pseudocode
    ```
    for i := 1..n
        if Arr[abs(i)] > 0
            // mark it negative
            Arr[abs(i)] = -Arr[abs(i)]
        else
            // Arr[abs(i)] is negative, we revisit integer i
            integer i is a repetition
    ```