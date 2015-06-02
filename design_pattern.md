# Design Pattern

## Singleton Pattern
Ensures that only one instance of a class is allowed within a system. It is useful to contain a very large read-only object.

5 modes of Singleton Pattern:
  1. lazy mode: create the object in get method. This will save memory. However, it is not thread-safe (threads may create multiple objects).
  2. thread-safe lazy mode, low performance: we make get method `synchronized`, but it will reduce performance. (only one thread can get the object at a time)
  3. greedy mode: Eager initialization - create the object when loading the class. Thread-safe, but need the memory at very beginning.
  4. (#6) (best) Thread-safe lazy mode with high performance: *double-checked locking* mechanism - add a `synchronized` block.

    ```java
    public class Singleton() {
        private static volatile Singleton instance;
        // private constructor,
        // prevent from creating instance using `new`
        private Singleton() {}
        public static Singleton getInstance() {
            if (instance == null) {
                synchronized (Singleton.class) {
                    if (instance == null) {
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }
    }
    ```

    **Note:** without `volatile`, there is a platform based bug: if the object is very big, initialization will take a long time to finish. Based on different OS, the half baked object will has a reference or doesn't has one yet. If we have the reference to access this half baked object, it throws `LazyInitializationException`.

  5. Using enum (Java 5 or above): `Enum` is thread-safe and implementation of Singleton by default. However, an enum cannot extends class or implements interfaces.

    ```java
    public enum SingleInstance {
        INSTANCE;
        private SingleInstance() {}
        public void doStuff() { ... }
    }

    // Use it like this
    SingleInstance.INSTANCE.doStuff();
    ```

### Why not just use all static fields to store data?
Static fields will use stack memory.

### Can it implements `Serializable`?
No, serialization will create a deep copy of the object which violates the pattern.

### Is there a way to create duplicate objects?
Yes, use reflection api.

### Which classes in JDK uses singleton pattern?
`java.lang.Runtime`. `getRuntime()` is equivalent to `getInstance()`.

### How to prevent for creating another instance using reflection?
Throws an Exception from constructor.

### How to prevent for creating another instance using serialization?
Return the same object in `readResolve()`. However, a enum singleton is taking cared by JVM by default.

## Immutable pattern


## Decorator Pattern
* Keyword: function enhancement.
* Type: Object Structural pattern
* Purpose: Allows for the dynamic wrapping of objects in order to modify their existing responsibilities and behaviors
* Decorator pattern is applied in 
  1. Java Input/Output API
  2. `Thread` class


## Prototype Pattern
* Keyword: clone, shallow copy
* Prototype 


## Adapter Pattern
* Keyword: function borrowing.
* Adapter pattern is applied in Stack Collection.


## Observer Pattern
* Type: Behavioral pattern
* Purpose: Lets one or more objects be notified of state changes in other objects within the system.


## Command Pattern


## Proxy Pattern


## Builder Pattern

