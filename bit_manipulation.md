# Bit Manipulation

### Count number of 1s in an integer
[Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)

* Java:

    ```java
    Integer.bitCount(int)
    ```

* `x & -x`: returns the least significant '1' bit of x.
* `-1 >> n`: returns -1, since -1 is represented by all '1' bits.