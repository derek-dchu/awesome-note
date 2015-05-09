# Language Basic

## Variables
* Instance Variables (Non-Static Fields)
* Class Variables (Static Fields)
* Local Variables
* Parameters


## Primitive Data Types
| Type | Length | Range | Default |  
|------|--------|-------|---------|   
| byte | 8-bit | -128 ~ 127 | 0 |  
| short | 16-bit | -32,768 ~ 32,767 | 0 |  
| int | 32-bit | -2^31 ~ 2^31-1 | 0 |  
| long | 64-bit | -2^63 ~ 2^63-1 | 0L |  
| float | 32-bit | single-precision | 0.0f |  
| double | 64-bit | double-precision | 0.0d |  
| boolean | no precisely defined | true/false | false |  
| char | 16-bit | '\u0000' ~ '\uffff' | '\u0000' |  

* Integer divided by zero throws `java.lang.ArithmeticExcaption`, while double divided by zero integer is Infinity, or divided by double zero is NaN, and both don't throw any exception.

**Note:** local variables never assigned a default value. Accessing an uninitialized local variable will result in compile-time error.


## Arrays
An array is a container object that holds a fixed number of values of a single type.

### Creating, Initializing
```java
// 1
int[] anArray = new int[10];
anArray[0] = 100;
anArray[1] = 200;

// 2
int [] anArray = {100, 200};
```


## Operations
### Logical and Bitwise
* Priority: `!` > `&&` > `||`.
* `&`, `|`: both sides will be checked.

## Control Flow
### switch statement
`switch` works with `byte`, `short`, `char`, `int`, `Enum`, `Character, `Short, `Byte`, `Integer`, `String` (Java 7).

**Note:** if expression in any switch statement is `null`, then a `NullPointerException` is thrown.