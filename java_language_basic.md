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


## Operators
Operator Precedence

| Operators | Precedence |
| --------- | ---------- |
| postfix | expr++ expr-- |
| unary | ++expr --expr +expr -expr ~ !
| multiplicative | * / % |
| additive | + - |
| shift | << >> >>> |
| relational | < > <= >= instanceof |
| equality | == != |
| bitwise AND | & |
| bitwise exclusive OR | ^ |
| bitwise inclusive OR | &#124; |
| logical AND | && |
| logical OR | &#124;&#124; |
| ternary | ? : |
| assignment | = += -= *= /= %= &= ^= &#124;= <<= >>= >>>= |

### Relational
Because `null` is not an instance of anything, `null instanceof anything` always return false.

### Logical and Bitwise
* Priority: `!` > `&&` > `||`.
* `&`, `|`: both sides will be checked.
* `>>`: signed right shift operator that shifts a bit pattern to the right by extending the sign bit with the number of positions to shift by the right-hand operand. 
    * `-1 >> n` => `-1`.
* `<<`: signed left shift operator.
* `>>>`: unsigned right shift operator that shifts **a zero** into the leftmost position.


## Control Flow
### switch statement
`switch` works with `byte`, `short`, `char`, `int`, `Enum`, `Character, `Short, `Byte`, `Integer`, `String` (Java 7).

**Note:** if expression in any switch statement is `null`, then a `NullPointerException` is thrown.