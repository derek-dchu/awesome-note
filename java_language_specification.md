# Language Specification

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

## Enum Types
**Note:** all enums implicitly extend `java.lang.Enum`, therefore it cannot extend anything else.


## Control Flow
### switch statement
`switch` works with `byte`, `short`, `char`, `int`, `Enum`, `Character, `Short, `Byte`, `Integer`, `String` (Java 7).

**Note:** if expression in any switch statement is `null`, then a `NullPointerException` is thrown.


## Number Class
An final and immutable wrapper classes for each of the numeric primitive data types.


## Character Class
An final and immutable wrapper class for `char`.


## String
**Note:** String is final, immutable, compareable, serializable.

* String implements `CharSequence`.

### HashCode
```
for each character c in string:
	hashCode = c + 31 * hashCode
```

* Why choose 31?
  1. prime number: evenly distributed

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
If the value p being boxed is `true`, `false`, a `byte`, or a `char` in the range `\u0000` to `\u007f`, or an `int` or `short` number between -128 and 127 (inclusive), then let `r1` and `r2` be the results of any two boxing conversions of p. It is always the case that `r1 == r2`.


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


## Generics
Generics enable `types` (classes and interfaces) to be parameters when defining classes, interfaces and methods. It is used in collection api, class, function.

> Without generics, collection api, some classes, some functions will return Object, so that we need to downcast to proper class. If we use generics, it limits to a specified type, that is why it is type-safe.

* Stronger type checks at compile time (Type safe), thus reducing errors at runtime.
* Elimination of casts.
* Generic algorithms.

### Generic Methods
Methods that introduce their own type parameters.

* For static generic method, the type parameter section must appear before the method's return type.

```java
public staic <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) { /* ... */ }
```

### Multiple Bounds
* Class must be specified first.

```java
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }

class E <T extends B & A & C> { /* ... */ }  // compile-time error
```

### WildCard
It represents an unknown type.

* What is the different between Object and WildCard (?)

```java
List<String> list = new ArrayList<String>();
List<Object> list2 = list;	// compile error
List<?> list3 = list;		// works
```
Because, although `String` is a `Object`, `List<String>` is NOT a `List<Object>`. However, every `List` is a `List<?>`

### `extends` vs `super`
`List<? extends A> list`: itself and subclass

`List<? super A>list`: itself and superclass

> **Note:** class A can be an interface

### Wildcard Guidelines: 
`method(in, out)`

* in: `extends`.
* out: `super`.
* In the case where the "in" variable can be accessed using methods defined in the Object class, use an unbounded wildcard.
* In the case where the code needs to access the variable as both an "in" and an "out" variable, do not use a wildcard.

### Type Erasure
Instead of creating new classes
* Replace all type parameters.
* Insert type casts.
* Generate bridge methods.


## Immutable vs Final 
### Immutable
value can not be changed.

** (#1) How to define an immutable class:**
* Declare class as final OR make the constructor private and construct instances in factory methods
* All fields are `private` and `final`
* Constructor for initialization
* No setters - methods that modify fields or objects referred to by fields.
* If the instance fields include references to mutable objects, don't allow those objects to be changed:
  1. don't provide methods that modify the mutable objects.
  2. don't share references to the mutable objects. Never store references to external, create copies in constructor. Similarly, create copies of your internal mutable objects in getter.
* If the field is a collection, use `Collections.unmodifiableXXX(XXX collection)` to change it into a unmodifiable collection.

Benefits: Immutable object is thread-safe and commonly used in multi-threading environment.

### Final
The modifier prohibiting value modification.


## Upcast vs Downcast
### Upcast  
* Primitive: shorter byte to longer byte.
* Objects: upcast by "is a".

### Downcast
* Primitive: longer byte to shorter byte.  
* Objects: downcast from parent reference to child.

### Conversion:
* Primitive
  1. closest upper primitive type
  2. wrapper class  
  
* Class  
closest upper class in the inheritance tree. If there are more than one class available, there will be a compile error.


## Java file
### .java file without file name
It is valid. It can be compiled and run with inner public class name.

### Java file without a public class
It runs normally. If a class has the same name as Java file name, it is default to be public.


## Main Function 
* Without public or static: Runtime error.
* Can be overloaded.
* Can be final.


## Import
* Package/class can imported multiple times but JVM will load only once.
* Static import: access static method without qualify it by the class name.


## final, finally, finalize
### final
Initialize final variable
```java
// 1
class A {
	final int x = 10;
}

// 2
class A {
	final int x;
	A(int x) { this.x = x; }
}

// 3
// cannot assign a value to final variable x
class A {
	final int x;
	A(int x) { init(x); }
	void init(int x) { this.x = x; }
}
```
Only 1 & 2 can be used. A final variable that is not initialized at the time of declaration which can be initialize only in constructor.

* final object still can change value. (e.g. Array)
* final method cannot be overridden.
* final class cannot be inherited.

### finally {}
`finally` block will not be executed if program exits `system.exit()` or fatal error (e.g. OutOfMemoryError).

### finalize()
`finalize` is a method can be called by the garbage collector on an object when garbage collection determines that there are no more references to the object. It performs like a destructor in C++. It can be override.


## Garbage Collection
Garbage Collection is the process of looking at heap memory, identifying which objects are in use and which are not, and deleting the unused objects.

* Garbage Collection is performed by a daemon thread called Garbage Collector(GC) with the lowest priority.
* GC calls the finalize() method before object is garbage collected once and only once.
* How to invoke garbage collection
  1. `System.gc();`
  2. `Runtime.getRuntime().gc();`
* GC cannot be forced to execute.
* A lost reference might be obtained back (object resurrection) and the object will not be GCed any more which might result memory leak.
* GC only collects those objects that are created by `new` keyword.

### When an object becomes eligible for Garbage Collection
1. If it is not reachable from any live thread.
2. If it is not reachable by any strong references.
3. Cyclic dependencies are not counted as reference. e.g. A -> B, B -> A, both A and B are eligible.

### Garbage Collection Procedures
Step 1: Marking
Scan memory to find unreferenced objects.

Step 2: Normal Deletion
The memory allocator removes unreferenced objects and holds references to blocks of free space.

Step 2a: Deletion with Compacting
To further improve performance, in addition to deleting unreferenced objects, you can also compact the remaining referenced objects. By moving referenced object together, this makes new memory allocation much easier and faster.

### JVM Generations
THe heap is broken up into smaller parts: Young, Tenured, and Permanent Generation.

Young Generation: all new objects are allocated and aged. When it fills up, this causes a *minor garbage collection*.

**Note:** Stop the World Event: all minor garbage collections are always "Stop the World" events, which means all application threads are stopped until the operation completes.

Old Generation: when a object in Young generation meets a threshold age, it will be removed to here. However, if a object can not be allocated in Eden space even after minor garbage collection, it will be allocated in here. When it fills up, this causes a *
major garbage collection*.

**Note:** major garbage collection are also Stop the World events. Often it is much slower and should be minimized.

Permanent generation: contains metadata required by the JVM to describe the classes and methods used in the application. The permanent generation is populated by the JVM at runtime based on classes in use by the application. In addition, Java SE library classes and methods may be stored here. Classes may get collected (unloaded) if the JVM finds they are no longer needed and space may be needed for other classes. The permanent generation is included in a full garbage collection.

### Questions:
#### 1. Create an object that can not be garbage collected
```java
class A {
  static final Object obj = ... ;
}
```

#### 2. How to minimize garbage collection?
Object Pooling, i.e. Factory Pattern.


## Reference Types
### Strong References
Regular object reference: not eligible for GC.

### Weak Reference (`java.lang.ref.WeakReference<T>`)
`WeakReference<Cacheable> weakData = new WeakReference<Cacheable>(data);`

Will be collected at the next GC cycle. A example of it is `WeakHashMap`.

### Soft Reference (`java.lang.ref.SoftReference<T>`)
Like a weak reference but it is less likely to be GC. Soft references are cleared at the discretion of the garbage collector in response to memory demand.

### Phantom Reference (`java.lang.ref.Phantom<T>`)
* Cannot be used to retrieve the object.
* It gets enqueued into a `ReferenceQueue` when the object is physically removed.
* It is the only way to determine exactly when an object was removed from memory.


## Multi-threading


## Lazy-loading
Abstract class don't have Lazy-loading feature


## instanceof vs getClass()
```java
class A
class B extends A
class C extends B

A a = new C()
B b = (B) a;
C c = (C) a;

a instanceof A, B, C // true
b instanceof A, B, C // true
c instanceof A, B, C // true

a,b,c getClass() // C
```
If an object is `instanceof` of class, that means the object can do either upcasting or downcasting to that class.


## Serializable
### What is a serialiable object?
A serialiable object can be converted into a binary string, so that it can be transfered via Internet or saved to a file.

And it is done by `ObjectOutputStream.writeObject(obj)` and `ObjectOutputStream.readObject()`.

### Serialization vs Deserialization
Serialization: convert a serializable object into a binary stream which can be persisted into disk or sent over network to any other running JVM.

* For an class, each level of fields need to be serializable (regarding `transient`) to perform serialization.
* For an class, it must has at less one superclass that is serializable to perform serialization.

Deserialization: the reverse process of serialization.

* Deserialization will construct the object directly without calling its constructor. However, if the class is subclass, then it will call constructors of its superclass which are not serializable.

### Override Default Serialization
We can provide following method to override default serialization:
  1. `private void writeObject(java.io.ObjectOutputStream out) throws IOException`
  2. `private void readObject(java.io.ObjectInputputStream in) throws IOException`

And why we want to override it?
Because if super class become serializable, we can throw `NotSerializableException` to prevent current class from serialization.

### What is the purpose of `SerialVersionUID`?
It is a `private static final long` variable. It is a class uuid that guarantees serialization and deserialization are performed on the same class.

If deserialize an object with a different `SerialVersionUID`, Java Serialization API will throw `java.io.InvalidClassException`.

**note:** If `SerialVersionUID` is not provided, normally system will use hashCode instead. 

### `transient` keyword
Make a field non-serialiable in a serialiable class.

* We can put `transient` in any class.
* We can put `transient` in any field.
* Static fields cannot be serialize, because it is implicitly `transient`.

### Create a deep copy using serializable
**note:** If an object implements serializable at each level, then deep copy of the object can be made via serializable.

### What will happen if one of the members in the class doesn't implement Serializable interface?
Throws `NotSerializableException` at runtime.


## Externalizable
It is a subinterface of Serialiable which contains two methods: 
  1. `void readExternal(ObjectInput in)`
  2. `void writeExternal(ObjectOutput out)`.

  **note:** `readExternal` and `writeExternal` supersede any specific implementation of `writeObject` and `readObject` methods.

It provides complete control on format and content of Serialization process to application which can be leverage to increase performance and speed of serialization process. (e.g. encryption)

However, it is not flexible due to the complete responsibility to control serialization when we change the class definition.

* `transient` has no effect in externalizable object.
* externalizable class will only invoke default constructor of the object (no invocation of superclass constructors) to deserialize.


## Annotations
Annotations, a form of metadata, provide data about a program that is not part of the program itself.

* if there is just one element named `value`, then the name can be omitted.
* Java 8 supports repeating annotations (applying same types of annotations)
* Java 8 supports **type annotation**.
* annotation is form of interface.

### Declaring an Annotation Type
```java
@interface Name {
	String firstName();
	String lastName() default "N/A";
}
```

## Platform Environment
### Properties
* extends Hashtable -> Dictionary
* Life Cycle:
  1. Starting Up  
  
  ```java
  // create and load default properties
  Properties defaultProps = new Properties();
  FileInputStream in = new FileInputStream("defaultProperties");
  defaultProps.load(in);
  in.close();

  // create application properties with default
  Properties applicationProps = new Properties(defaultProps);

  // load properties from last invocation
  in = new FileInputStream("appProperties");
  applicationProps.load(in);
  in.close();
  ```

  2. Running
    1. `contains(Object value)`, `contains(Object key)`
    2. `getProperty(String key)`, `getProperty(String key, String default)`
    3. `list(PrintStream s)`, `list(PrintWriter w)`
    4. `keys()`: keys for itself, `propertyNames()`: keys for itself and default(inherited keys). Both returns `Enumeration`
    5. `stringPropertyNames()`: returns names of properties where both key and value are strings. The `Set` object is not backed by the `Properties` object.
    6. `size()`
    7. `setProperty(String key, String value)`
    8. `remove(Object key)`

  3. Exiting
  ```java
  // save application properties
  FileOutputStream out = new FileOutputStream("appProperties");
  applicationProps.store(out, "---No Comment---");
  out.close();
  ```

### Environment Variables
* `System.getenv()` returns a read-only `Map` where the map keys are the environment variable names, and values are the environment variable values.
* `System.getenv(String varName)` returns the value for environment variable named varName or returns null if it is not defined. **Note:** To maximize portability, never refer to an environment variable when the same value is available in a system property.
* Create a new process: `ProcessBuilder`.

### System Properties
`System` class maintains a Properties object that describes the configuration of the current working environment.

* The runtime system re-initializes the system properties each time its starts up. That is, changing the system properties within an application will not affect future invocations of the Java interpreter for this or any other application.


## CLASSPATH variable
The CLASSPATH variable is one way to tell applications, including the JDK tools, where to look for user classes.

* default value: "."
* Using `-cp`, `-classpath` command line switch to override default value for each application without affecting other applications.
* We can also set CLASSPATH directly.



