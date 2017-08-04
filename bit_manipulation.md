# Bit Manipulation

## Basic Operation

* `x & -x`: returns the least significant '1' bit of x.
* `-1 >> n`: returns -1, since -1 is represented by all '1' bits.
* `flags &= ~MASK`: reset the kth bit flag within flags using MASK equals to 100...00 \(k length\).
* `(flags & MASK) != 0`: check if the kth bit is set. If it is set, return true.
* `x = x & (x-1)` : from right to left, jump to next '1' bit of x.

### Count number of 1s in an integer

[Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)

* Java:

  ```java
    Integer.bitCount(int)
  ```

### O\(1\) Check Power of 2

Using O\(1\) time to check whether an integer n is a power of 2.

##### Analysis

Check whether its binary form is zeros leading with a single '1' bit.

##### Solution

```
n & (n - 1) == 0
```

### Swap two integers without using the third variable

```
a ^= b
b ^= a
a ^= b
```

### Number of 1 Bits

Write a function that takes an unsigned integer and returns the number of ’1' bits it has \(also known as the[Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)\).

For example, the 32-bit integer ’11' has binary representation`00000000000000000000000000001011`, so the function should return 3.

##### Solution

```python
count = 0
while x:
    count += 1
    x = x & (x - 1) # e.g. 7 -> 6 -> 4 -> 0
```



