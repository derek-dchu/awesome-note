# Language Specification

## Java file
### .java file without file name
It is valid. It can be compiled and run with inner public class name.

### Java file without a public class
It runs normally. If a class has the same name as Java file name, it is default to be public.


## The `main` Method
*  Every application must contain a main method whose signature is:

    ```java
    public static void main(String[] args)
    ```
    

*  Without public or static: Runtime error.
*  Can be overloaded.
*  Can be final.


## Import
*  Package/class can imported multiple times but JVM will load only once.
*  Static import: access static method without qualify it by the class name.
*  [JAR hell](http://en.wikipedia.org/wiki/Java_Classloader#JAR_hell): if we have different versions of jar files on the same class path, the classloader will load the first one on the class path. The class we import will come from that version of jar file.


## final, finally, finalize
### The `final` keyword
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
// error: cannot assign a value to final variable x
class A {
	final int x;
	A(int x) { init(x); }
	void init(int x) { this.x = x; }
}
```
Only 1 & 2 can be used. A final variable that is not initialized at the time of declaration, can be initialize only in constructor.

* final object still can change value. (e.g. Array)
* final method cannot be overridden.
* final class cannot be inherited.

### The `finally` block
`finally` block will not be executed if program exits `system.exit()` or fatal error (e.g. OutOfMemoryError).

### The `finalize` method
`finalize` is a method can be called by the garbage collector on an object when garbage collection determines that there are no more references to the object. It performs like a destructor in C++.

*  Can be overridden.
*  If an uncaught exception is thrown by the finalize method, the exception is ignored and finalization of that object terminates.
*  The finalize method is never invoked more than once by a Java virtual machine for any given object.


## Garbage Collection
Garbage Collection is the process of looking at heap memory, identifying which objects are in use and which are not, and deleting the unused objects.

*  Garbage Collection is performed by a daemon thread called Garbage Collector (GC) with the lowest priority.
*  GC calls the finalize() method before object is garbage collected once and only once.
*  How to invoke garbage collection
  1. `System.gc();`
  2. `Runtime.getRuntime().gc();`
*  GC cannot be forced to execute.
*  A lost reference might be obtained back (object resurrection) and the object will not be GCed any more which might result memory leak.
*  GC only collects those objects that are created by `new` keyword.

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
The heap is broken up into smaller parts: Young, Tenured, and Permanent Generation.

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



