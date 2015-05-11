# Binary Search

### Sqrt(x)
Implement int sqrt(int x) method which returns the first integer that is smaller than or equals to, if exists, square root of x.

For example: 

sqrt(3) = 1  
sqrt(4) = 2

Solution:

```
def sqrt(integer x)
    integer l := 0
    integer r := x + 1 # add 1 for the case that x = 1
    while r - l > 1
        integer mid = (r - l) / 2 + l # prevent overflow
        
        # we are doing integer multiplication instead of float here
        if mid * mid <= x
            l := mid
        else
            r := mid
    return l
```

**Note:** because the method returns an integer, we don't need to perform float multiplication which is much slower than integer multiplication.