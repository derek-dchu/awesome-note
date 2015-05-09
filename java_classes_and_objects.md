# Classes and Objects

## Enum Types
**Note:** all enums implicitly extend `java.lang.Enum`, therefore it cannot extend anything else.


## Number Class
An final and immutable wrapper classes for each of the numeric primitive data types.


## Character Class
An final and immutable wrapper class for `char`.


## String
**Note:** String is final, immutable, compareable, and serializable.

* String implements `CharSequence`.

### HashCode
```
for each character c in string:
	hashCode = c + 31 * hashCode
```

* Why choose 31?
  1. prime number: hash code tend to be evenly distributed

### String pool and intern()  
There is a string pool in stack which contains non-duplicated strings. Those strings will be reused by references. `intern()` returns canonical copy of string from the string pool. Create a string using new keyword will firstly create a string in string pool and then copy it into heap to create an String object.

### String vs StringBuffer vs StringBuilder  
* String is immutable.  
* StringBuffer, StringBuilder are mutable.  
* StringBuffer is thread-safe.  
* StringBuilder (Java 5) is not thread-safe which has better performance.

> **Note:** thread-safe means:
>    1. the class is thread-safe: only one thread can access that class at a time.
>    2. the program is thread-safe: return consistence output.


## Constant pool
If the value p being boxed is `true`, `false`, a `byte`, or a `char` in the range `\u0000` to `\u007f`, or an `int` or `short` number between -128 and 127 (inclusive), and let `r1` and `r2` be the results of any two boxing conversions of p, then we will have `r1 == r2`.


## Autoboxing vs Unboxing
Both occurs at compile time.

### Autoboxing (Java 5)
Automatic conversion from primitive type to corresponding wrapper class.

```java
Integer x = 10; int y = x;

List<Integer> li = new ArrayList<>();
li.add(10);
```

* Passed as a parameter to a method that expects an object of the corresponding wrapper class.
* Assigned to a variable of the corresponding wrapper class.

### Unboxing
reverse operation of autoboxing.
