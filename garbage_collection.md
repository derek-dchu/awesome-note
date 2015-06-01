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

*  Young Generation: all new objects are allocated and aged. When it fills up, this causes a *minor garbage collection*.

    **Note:** Stop the World Event: all minor garbage collections are always "Stop the World" events, which means all application threads are stopped until the operation completes.

*  Old Generation: when a object in Young generation meets a threshold age, it will be removed to here. However, if a object can not be allocated in Eden space even after minor garbage collection, it will be allocated in here. When it fills up, this causes a *
major garbage collection*.

    **Note:** major garbage collection are also Stop the World events. Often it is much slower and should be minimized.

*  Permanent generation: contains metadata required by the JVM to describe the classes and methods used in the application. The permanent generation is populated by the JVM at runtime based on classes in use by the application. In addition, Java SE library classes and methods may be stored here. Classes may get collected (unloaded) if the JVM finds they are no longer needed and space may be needed for other classes. The permanent generation is included in a full garbage collection.

### Questions:
#### 1. Create an object that can not be garbage collected
```java
class A {
  static final Object obj = ... ;
}
```

#### 2. How to minimize garbage collection?
Object Pooling, i.e. Factory Pattern.