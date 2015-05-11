# Generics
Generics enable `types` (classes and interfaces) to be parameters when defining classes, interfaces and methods. It is used in collection api, class, function.

> Without generics, collection api, some classes, some functions will return Object, so that we need to downcast to proper class. If we use generics, it limits to a specified type, that is why it is type-safe.

* Stronger type checks at compile time (Type safe), thus reducing errors at runtime.
* Elimination of casts.
* Generic algorithms.

## Generic Methods
Methods that introduce their own type parameters.

* For static generic method, the type parameter section must appear before the method's return type.

```java
public staic <K, V> boolean compare(Pair<K, V> p1, 
                                Pair<K, V> p2) { /* ... */ }
```

## Multiple Bounds
* Class must be specified first.

```java
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }

// compile-time error
class E <T extends B & A & C> { /* ... */ }
```

## WildCard
It represents an unknown type.

* What is the different between Object and WildCard (?)

    ```java
    List<String> list = new ArrayList<String>();
    List<Object> list2 = list;  // compile error
    List<?> list3 = list;       // works
    ```
    Because, although `String` is a `Object`, `List<String>` is NOT a `List<Object>`. However, every `List` is a `List<?>`

### `extends` vs `super`
* `List<? extends A> list`: A or its subclass
* `List<? super A> list`: A or its superclass
* `extends` and `super` cannot be used at the same time

  **Note:** A can be class or interface

### Wildcard Guidelines: 
`method(in, out)`

* An in variable is defined with an upper bounded wildcard, using `extends`.

  > If a collection referenced by Collection<? extends ...>, it can be informally thought of as read-only (cannot store a new element or change an existing element without knowing the exact class when it is originally defined).
  > ```java
  > B extends A
  > List<B> listB = new ArrayList<>();
  > List<? extends B> list = listB;
  > list.add(new A));     // compile-time error
  > ```

* An out variable is defined with a lower bounded, using `super`.
* In the case where the "in" variable can be accessed using methods defined in the Object class, use an unbounded wildcard.
* In the case where the code needs to access the variable as both an "in" and an "out" variable, do not use a wildcard.
* Using a wildcard as a return type should be avoided.

## Type Erasure
Instead of creating new classes
1. Replace all type parameters.
2. Insert type casts.
3. Generate bridge methods.