# Design Pattern

## Singleton Pattern
Ensures that only one instance of a class is allowed within a system. It is useful to contain a very large read-only object.

5 modes of Singleton Pattern:
  1. lazy mode: create the object in get method. This will save memory. However, it is not thread-safe (threads may create multiple objects).
  2. thread-safe lazy mode, low performance: we make getInstance method `synchronized`, but it will reduce performance. (only one thread can get the object at a time)
  3. greedy mode: Eager initialization - create the object when loading the class. Thread-safe, but allocate the memory at very beginning.

    ```java
    public class EagerSingleton {
        private static volatile EagerSingleton instance = new EagerSingleton();
        private EagerSingleton() {}
        public static EagerSingleton getInstance() {
            return instance;
        }
    }
    ```

  4. (#6) (best) Thread-safe lazy mode with double check: *double-checked locking* mechanism - add a `synchronized` block.

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
  6. Inner static class initialization. It is lazy loaded, because inner static class is lazy loaded.
  
    ```java
    public class BillPughSingleton {
        private BillPughSingleton() {}
        private static class LazyHolder {
            private static final BillPughSingleton INSTANCE = new BillPughSingleton();
        }
        public static BillPughSingleton getInstance() {
            return LazyHolder.INSTANCE;
        }
}
    ```

### Why not just use all static fields to store data?
Static fields will use stack memory.

### Can it implements `Serializable`?
Yes, but serialization will create a deep copy of the object which violates the pattern, so it is not recommended. If we have to implements `Serializable`, we have to override `readResolve()` so that it returns the instance.

### Is there a way to create duplicate objects?
Yes, use reflection api.

### Which classes in JDK uses singleton pattern?
`java.lang.Runtime`. `getRuntime()` is equivalent to `getInstance()`.

### How to prevent for creating another instance using reflection?
Throws an Exception from constructor.

### How to prevent for creating another instance using serialization?
Return the same object in `readResolve()`. However, a enum singleton is taking cared by JVM by default.


## Factory Pattern
* Type: Object Creational
* JDK usage:
  1. java.sql.DriverManager#getConnection()
  2. java.lang.Class#newInstance()


## Immutable pattern


## Decorator Pattern
* Keyword: function enhancement.
* Type: Object Structural pattern
* Purpose: Allows for the dynamic wrapping of objects in order to modify their existing responsibilities and behaviors
* JDK usage:
  1. Java Input/Output API
  2. `Thread` class

### Components
* Component: this is the wrapper which can have additional responsibilities associated with it at runtime.
* Concrete component: is the original object to which the additional responsibilities are added in program.
* Decorator: this is an abstract class which contains a reference to the component object and also implements the component interface.
* Concrete decorator: it extends the decorator and builds additional functionality on top of the Component class.


## Prototype Pattern
* Keyword: clone, shallow copy
* Prototype 


## Adapter Pattern
* Type: Structural design pattern.
* It is used to make two unrelated interfaces work together.
* JDK usage: Stack.

### Components
* Target: It defines the application-specific interface that Client uses directly.
* Adapter: It adapts the interface Adaptee to the Target interface. It’s middle man.
* Adaptee: It defines an existing incompatible interface that needs adapting before using in application.
* Client: It is your application that works with Target interface.


## Observer Pattern
* Type: Behavioral pattern
* Purpose: Lets one or more objects be notified of state changes in other objects within the system.


## Command Pattern


## Proxy Pattern


## Builder Pattern
* Type: Object creation design pattern
* Could be used to solve the problem that construct an object with many optional parameters. (usually we need to overload constructors for all kinds of combinations). E.g. builder for immutable class.
* JDK usage: StringBuilder.
