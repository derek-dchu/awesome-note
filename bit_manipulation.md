# Bit Manipulation

### Count number of 1s in an integer
[Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)

* Java:

    ```java
    Integer.bitCount(int)
    ```
    
### O(1) Check Power of 2
Using O(1) time to check whether an integer n is a power of 2.

##### Anaylsis  
Check whether its binary form is zeros leading with a single '1' bit.

##### Solution  
```
n & (n - 1) == 0
```


* `x & -x`: returns the least significant '1' bit of x.
* `-1 >> n`: returns -1, since -1 is represented by all '1' bits.
* flags &= ~MASK: reset the kth bit flag within flags using MASK equals to 100...00 (k length).