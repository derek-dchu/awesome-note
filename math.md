# Math

### Unique Paths

A robot is located at the top-left corner of a m x n grid \(marked 'Start' in the diagram below\).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid \(marked 'Finish' in the diagram below\).

How many possible unique paths are there?

##### Analysis

For the robot, to reach bottom-right, it has to perform m-1 of moving right and n-1 of moving down, and the order of moves don't matter. Therefore, total unique paths equals to $$C(^{(m-1)+(n-1)}_{m})$$ or $$C(^{m+n-2}_{n})$$.

### Trailing Zeros

Write an algorithm which computes the number of trailing zeros in n factorial with log\(n\) complexity.

Example  
11! = 39916800, so the out should be 2

##### Analysis

Fact: if we look at prime factors of each number in 1 .. n, only the combination of 2, 5 will produce a trailing zero, and 2 occurs much more times than 5. In other words, the number of prime factor 5 equals to the number of trailing zeros.

##### Pseudocode

```
count := 0
while n > 0
    n := n / 5
    count := count + n
return count
```

### Unique Binary Search Trees

Given n, how many structurally unique BSTs \(binary search trees\) that store values 1...n?

##### Analysis

It can be easily found that   
$$C(n) = \sum_{k=1}^nC(k-1) * C(n-k)$$  
where $$C(n)$$ equals the number of structurally unique BSTs for n numbers.

Above formular represents the fact that for n numbers, the number of structurally unique BSTs equals to summation of all senarios in each which a number is choosen to be the root, all numbers smaller form a left subtree, all numbers greater form a right subtree.

##### Pseudocode

```
# 0 number: 1
# 1 number: 1
cache := {1,1,2}
for i in 2 .. n
    c := 0
    for k in 1 .. i
        c := c + cache[k-1] * cache[i-k]
    cache[i] = c
return cache[n]
```

### Pow\(x,n\)

[Exponentiation by squaring](http://en.wikipedia.org/wiki/Exponentiation_by_squaring)

### Modulo Exponentiation

Calculate the $$a^n \mod b$$ where a, b and n are all 32bit integers.

Example  
$$2^{31} \mod 3 = 2$$

$$100^{1000} \mod 1000 = 0$$

#### Modular arithmetic

$$(A \pm B) \% C = (A \% C \pm B \% C) \% C$$  
$$(A * B) \% C = (A \% C * B \% C) \% C$$  
$$A^B \% C = ( (A \% C)^B ) \% C$$

### Huge Fibonacci Number modulo m

Given two integers 𝑛 and 𝑚 , output 𝐹𝑛 mod 𝑚 \(that is, the remainder of 𝐹𝑛 when divided by 𝑚 \).

#### Solution

for any integer 𝑚 ≥ 2 , the sequence 𝐹𝑛 mod 𝑚 is periodic. The period always starts with 0,1 and is known as [Pisano period](https://en.wikipedia.org/wiki/Pisano_period).

example: 𝐹\(2015\) mod 3 = 𝐹\(7\) mod 3 = 1, because F mod 3 has period 8, and 2015 mod 8 is 7.



