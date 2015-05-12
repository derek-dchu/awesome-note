# Math

### Trailing Zeros
Write an algorithm which computes the number of trailing zeros in n factorial with log(n) complexity.

Example
11! = 39916800, so the out should be 2

#### Anaylsis  
Fact: if we look at prime factors of each number in 1 .. n, only the combination of 2, 5 will produce a trailing zero, and 2 occurs much more times than 5. In other words, the number of prime factor 5 equals to the number of trailing zeros.

#### Pseudocode
```
count := 0
while n > 0
    n := n / 5
    count := count + n
return count
```
