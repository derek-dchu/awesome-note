# Concurrency
In concurrent programming, there are two basic units of execution: processes and threads. For single execution core, it have one thread actually executing at any given moment. Processing time for a single core is shared among processes and threads through an OS feature called *time slicing*.

## Processes
A process has a self-contained execution environment. A process generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space.

To facilitate communication between processes, most operating systems support Inter Process Communication (IPC) resources, such as pipes and sockets.

Most implementations of the Java virtual machine run as a single process. A Java application can create additional processes using a `ProcessBuilder` object.

## Threads
Threads are sometimes called lightweight processes. Both processes and threads provide an execution environment, but creating a new thread requires fewer resources than creating a new process.

Threads exist within a process — every process has at least one. Threads share the process's resources, including memory and open files. This makes for efficient, but potentially problematic, communication.

For a application programmer, an application starts with just one thread, called the main thread. This thread has the ability to create additional threads.

## Thread Objects
Each thread is associated with an instance of the class Thread. There are two basic strategies for using Thread objects to create a concurrent application.

* To **directly control** thread creation and management, simply instantiate Thread each time the application needs to initiate an asynchronous task.
* To **abstract** thread management from the rest of your application, pass the application's tasks to an executor.

### `Thread` class
* Implements `Runnable` interface
* Contains a `Runnable` object
* Follows Decorator pattern
* `static Thread Thread.currentThread()`: returns a reference to the currently executing thread object. Commonly used by a Runnable object to retrive information of its executing thread.

### How to create and start a thread
An application that creates an instance of Thread must provide the code that will run in that thread.

1. Provide a Runnable object.

    ```java
    class A implements Runnable {
    	@Override
    	public void run() { ... }
    }
    
    Runnable a = new A();
    new Thread(a).start();
    ```
    
2. Subclass Thread.

    ```java
    class B extends Thread {
	    @Override
	    public void run() { ... }
    }

    Thread t = new B();
    t.start();
    ```

Method 1 is more flexible than method 2, because A can still extend another class.

* Both `a.run()`, `b.run()` can be called directly, but it is called by current thread.
* Calling `start()` will:
    1. create a separate thread
    2. call `run()` method
* `start()` can be only called once, or it throws a `IllegalThreadStateException`, because it cannot be reset from DEAD to READY.

### How to stop a thread
Doesn't support. Because stop a thread during running may lead to memory leak.

### Thread Lifecycle
Four stages:
1. READY:
  * to Running
2. RUNNING
  * back to Ready: `join()`, `yield()`
  * to Waiting: `sleep()`, `wait()`
  * to Dead
3. DEAD
4. WAITING
  * back to Ready: `notify()`, `notifyAll()`

```
		notify(), notifyAll()
Ready <------------------------- Waiting
 | ^                                ^
 | |join(), yield()                 |	
 v |								|
Running ----------------------------
 |			sleep(), wait()
 v
Dead
```

```java
public final void wait() throws InterruptedException;
public final void notify();
public final void notifyAll();
```

### `join` method
`void join()`

It is like a stop sign. When a thread calls `t.join()`, it push the thread into READY and wait for thread `t` to be terminated. `join` responds to an interrupt by exiting with an `InterruptedException`.

`void join(long millis)`,
`void join(long millis, int nanos)`: Waits at most millis milliseconds [+ nanos nanoseconds] for this thread to die.

### `yield` method
It is like a yield sign. Any thread calls `yield()` will make it back to READY status and wait for OS to pick it up.

* `yield()` is a static method.

### `sleep` method
`static void sleep(long millis)`,
`static void sleep(long millis, int nanos)`: millisecond + nanosecond.

Thread.sleep causes the current thread (the one invoke this method) to suspend execution for a specified period. So if thread a invokes thread b.sleep(), a is going to suspend, not b.

We cannot assume that invoking sleep will suspend the thread for precisely the time period specified.

**Note:** The sleep period can be terminated by interrupts, and an `InterruptedException` will be thrown.

### `interrupt` method
`void interrupt()`

Interrupts this thread.
An interrupt is an indication to a thread that it should stop what it is doing and do something else.

A thread sends an interrupt by invoking interrupt on the Thread object for the thread to be interrupted. For the interrupt mechanism to work correctly, the interrupted thread must support its own interruption (try ... catch InterruptedException or invoke Thread.interrupted() periodically).

#### The Interrupt Status Flag
The interrupt mechanism is implemented using an **internal flag** known as the **interrupt status**. Invoking Thread.interrupt sets this flag. When a thread checks for an interrupt by invoking the static method Thread.interrupted, interrupt status is cleared. The non-static isInterrupted method, which is used by one thread to query the interrupt status of another, does not change the interrupt status flag.

### `wait` method
Causes the current thread to wait until another thread invokes the notify() method or the notifyAll() method for this object. In other words, this method behaves exactly as if it simply performs the call wait(0). So `wait()` has no control.

* It is invoked on an object; current thread must synchronize on the lock object.
* If we use `wait()` without synchronized, it will throw `IllegalMonitorStateException` - the current thread is not the owner of the object's monitor.

**Note:** always invoke wait inside a loop that tests for the condition being waited for. Don't assume that the interrupt was for the particular condition you were waiting for, or that the condition is still true.

### `notify` method
Wakes up a single thread that is waiting on this object's monitor. It doesn't allow us to specify the thread that is woken up, it is useful only in massively parallel applications — that is, programs with a large number of threads, all doing similar chores. In such an application, you don't care which thread gets woken up.

* If we use `notify()` without synchronized, it will throw `IllealMonitorStateException`.

### `notifyAll` method
Wake up all threads that are waiting on this object's monitor. A thread waits on an object's monitor by calling one of the wait methods.


### Daemon Thread
Daemon Thread is a special thread that keeps running until all other threads stop and is forced to stop by OS.

* Garbage collection is a daemon thread.

#### Convert regular thread to daemon thread
```java
t.setDaemon(true);
t.start();
```

## Synchronization
Synchronization prevents two possible errors when threads are sharing access to fields and the objects reference fields refer to: *thread interference* and *memory consistency* errors.

Synchronization can introduce *thread contention*, which occurs when two or more threads try to access the same resource simultaneously and cause the Java runtime to execute one or more threads more slowly, or even suspend their execution.

Java provides two basic synchronization idioms: synchronized methods and synchronized statements.

### Thread Interference
interleave: different threads are acting on the same data.

### Memory Consistency
Memory consistency errors occur when different threads have inconsistent views of what should be the same data.

Can be solved by enforcing happens-before relationship that make sure data modification in thread A happens-before thread B.

### Synchronized Method
```java
synchronized method() { ... }
```
Making these methods synchronized using `synchronized` keyword has two effects:

  1. It is not possible for two invocations of synchronized methods on the same object to interleave. One thread executing a synchronzied method for an object, all other threads that invoke the same method are suspended until that thread is done with the object.

  2. When a synchronized method exits, it automatically establishes a happens-before relationship with any subsequent invocation of a synchronized method for the same object.

**Note:** constructor with `synchronized` is a syntax error.

### Synchronized Statements
Synchronized statements must specify the object that provides the intrinsic lock

```java
synchronized (obj) { ... }
```

### Intrinsic Locks and Synchronization
Synchronization is built around an internal entity known as the intrinsic lock or monitor lock (monitor).

* Every object has an intrinsic lock associated with it.
* A thread that needs exclusive and consistent access to an object's fields has to acquire the object's intrinsic lock before accessing them, and then release the intrinsic lock when it's done with them. The other thread will block when it attempts to acquire the lock.
* Methods don't have lock.
* To invoke a synchronized static method, the caller requires the intrinsic lock of the `Class` object associated with the class. (class level lock)
* To call a synchronized non-static method, the caller requires the intrinsic lock of that method's object. (object level lock)
* A class level lock can be converted to an object level lock.

    ```java
    class A {
    	static void foo2() {
    		synchronized(A.class) {
    			/* ... */
    		}
    	}
    }
    ```
### Reentrant Synchronization
A thread can acquire a lock that it already owns. Allowing a thread to acquire the same lock more than once enables *reentrant synchronization*, which avoids having a thread cause itself to block.

### Atomic Access
* Reads and writes are atomic for reference variables and for most primitive variables (all types except long and double).
* Reads and writes are atomic for all variables declared `volatile` (including long and double variables).

### `volatile` keyword
volatile is used to indicate that a variable's value will be modified by different threads.

* The value of this variable will never be cached thread-locally: all reads and writes will go straight to "main memory". Access to the variable acts as though it is enclosed in a synchronized block, synchronized on itself.
* If a thread modify a `volatile` object, this modification will be seen by all other threads immediately.
* `volatile` should be used to safely publish immutable objects in a multi-threading environment. Declaring a field like `public volatile ImmutableObject foo` secures that all threads always see the currently available instance reference.


## Liveness
A concurrent application's ability to execute in a timely manner is known as its liveness.

### Deadlock
Deadlock describes a situation where two or more threads are blocked forever, waiting for each other.

### Starvation
Starvation describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress.

### Livelock
A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then livelock may result.

### Guarded Blocks
Guarded block begins by polling a condition that must be true before the block can proceed. Block for a specific condition.

```java
public synchronized void guardedLock() {
    // This guard only loops once for each special event,
    // which may not be the event we're waiting for.
    while(!satisfied) {
        try {
            wait();
        } catch (InterruptedException e) {}
    }
    System.out.println("Job done!");
}

public synchronized notifyWaiter() {
    satisfied = true;
    notifyAll();
}
```


## Thread-safe
1. the class is thread-safe: only one thread can access that class at a time.
2. the program is thread-safe: return consistence output.


## Mutex


## Semaphore


## How to prevent dead lock
1. Use `Lock` class (safe lock)








## ThreadLocal
ThreadLocal is like an agent of shared resource. It wraps shared resource, and different threads will have their own shallow copies of the shared resource from ThreadLocal.

```java
class ThreadLocal<V> {
	public V get();
	public void set(V value);
	protected V initialValue();
}
```

* non-blocking algorithm.

## Immutable Objects
An object is considered *immutable* if its state caonnot change after it is constructed.

Benefits:
*  Simple to construct, test, and use.
*  Thread-safe and commonly used in multi-threading environment.
*  Allow hashCode to use lazy initialization, and to cache its return value.
*  Do not need to be copied defensively when used as a field.
*  Can be used as Map keys and Set elements.

### (#1) How to define an immutable class:
* Declare class as final OR make the constructor private and construct instances in factory methods
* All fields are `private` and `final`
* Constructor for initialization
* No setters - methods that modify fields or objects referred to by fields.
* If the instance fields include references to mutable objects, don't allow those objects to be changed:
  1. don't provide methods that modify the mutable objects.
  2. don't share references to the mutable objects. Never store references to external, create copies in constructor. Similarly, create copies of your internal mutable objects in getter.
* If the field is a collection, use `Collections.unmodifiableXXX(XXX collection)` to change it into a unmodifiable collection.

```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final Date birthday;
    private final List<ImmutablePerson> friends;

    public ImmutablePerson(String name, int age, Date birthday, 
                            List<ImmutablePerson> friends) {
        this.name = name;
        this.age = age;
        this.birthday = new Date(birthday.getTime());
        this.friends = Collections.unmodifiableList(friends);
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public Date getBirthday() {
        return new Date(birthday.getTime());
    }

    public List<ImmutablePerson> getFriends() {
        return friends;
    }
}
```


## High Level Concurrency Objects (JDK 5)
### Lock Objects
```java
package java.util.concurrent.locks
Interface Lock
```

Monitor lock forces all lock acquisition and release to occur in a block-structured way: when multiple locks are acquired they must be released in the **opposite order**, and all locks must be released in the **same lexical scope** in which they were acquired.

Implementations of the Lock interface allows a lock to be acquired and released in different scopes, and multiple locks to be acquired and released in any order.

In most cases, the following idiom should be used:
```java
Lock l = ...;
l.lock();
try {
    // access the resource protected by this lock
} finally {
    l.unlock();
}
```
* `boolean tryLock()`: a non-blocking attempt to acquire a lock.
* `void lockInterruptibly() throws InterruptedException`: an attempt to acquire the lock that can be interrupted.
* `boolean tryLock(long time, TimeUnit unit) throws InterruptedException`: an attempt to acquire the lock that can timeout
.

**Note:** Acquiring the monitor lock of a Lock instance has no specified relationship with invoking any of the lock() methods of that instance. It is recommended that never use Lock instances in this way.


## Executor Interfaces
* `Executor`: a simple interface that supports launching new tasks.

* `ExecutorService`: a subinterface of Executor, which adds features that help manage the lifecycle, both of the individual tasks and of the executor itself.

* `ScheduledExecutorService`: a subinterface of ExecutorService, supports future and/or periodic execution of tasks.

### The Executor Interface
The Executor interface provides a single method, `execute`, designed to be a drop-in replacement for a common thread-creation idiom.

```
(new Thread(r)).start(); --> executor.execute(r);
```

However, Runnable r could be run by an new thread or existing worker thread (most likely).

### The ExecutorService Interface
* supplements `Execute` with `submit` method which accepts `Runnable` objects, but also accepts `Callable` objects, which allow the task to return a value.

* provides a number of methods for managing the shutdown of the executor. To support immediate shutdown, tasks should handle interrupts correctly.

#### The `submit` method
```java
<T> Future<T> submit(Callable<T> task),
<T> Future<T> submit(Runnable task, T result),
<T> Future<T> submit(Runnable task)
```

It returns a `Future` object, which is used to retrieve the Callable return value and to manage the status of both Callable and Runnable tasks.

### The ScheduledExecutorService Interface
* supplements `ExecutorService` with `schedule` method, which executes a Runnable or Callable task after a specified delay.

* `scheduleAtFixedRate` and `scheduleWithFixedDelay`, which executes specified tasks repeatedly, at defined intervals.

#### The `schedule` method
```java
<V> ScheduledFuture<V> schedule(Callable<V> callable,
                            long delay, TimeUnit unit),
ScheduledFuture<?> schedule(Runnable command, 
                        long delay, TimeUnit unit)
```

### Thread Pools
Most of the executor implementations in java.util.concurrent use thread pools, which consist of worker threads.

* Minimizes the overhead due to thread creation (Thread objects use a significant amount of memory => significant memory management overhead).

* Types: fixed thread pool, cached thread pool.

#### Fixed Thread Pool
* Always has a specified number of threads running.
* Automatically replaced a terminated thread with a new thread.
* Tasks are submitted to the pool via an internal queue, which holds extra tasks whenever there are more active tasks than threads.
* Degrade gracefully: handle larget amount of tasks gracefully without overwhelming the system. For example, larget amount of HTTP requests.
* Creation: 

### The `java.util.concurrent.Executors` Factory Class
provides the following factory methods:
* `newFixedThreadPool`: creates an executor with a fixed thread pool.

* `newCachedThreadPool`: creates an executor with an expandable thread pool. This executor is suitable for applications that launch many short-lived tasks.

* `newSingleThreadExecutor`: creates an executor that executes a single task at a time.

* Several factory methods are ScheduledExecutorService versions of the above executors.

If none of the executors provided by the above factory methods meet the needs, instances of `java.util.concurrent.ThreadPoolExecutor` or `java.util.concurrent.ScheduledThreadPoolExecutor` have additional options.


### Fork/Join Framework
*  Implementation of the ExecutorService interface.
*  Takes advantage of multiple processors.
*  Work-stealing algorithm: worker threads that run out of things to do can steal tasks from other threads that are still busy.
*  `ForkJoinPool` implements the core work-stealing algorithm and can execute ForkJoinTask processes.
* Good examples:  `java.util.Arrays.parallelSort()`, methods in the `java.util.streams`.


## Concurrent Collections
*  Collections that help avoid Memory Consistency Errors by defining a happens-before relationship between an operation that adds an object to the collection with subsequent operations that access or remove that object.

### `BlockingQueue`
A first-in-first-out data structure that blocks or times out when you attempt to add to a full queue, or retrieve from an empty queue.

| | Throws exception | Special value | Blocks | Times out |
|-| ---------------- | ------------- | ------ | -------- |
| Insert | add(e) | offer(e) | put(e) | offer(e, time, unit) |
| Remove | remove() | poll() | take() | poll(time, unit) |
| Examine | element() | peek() | not applicable | not applicable |

* Not null elements.
* Can safely be used with multiple producers and multiple consumers.

*  `ConcurrentMap`: a subinterface of `java.util.Map` that defines atomic operations to remove or replace a key-value pair only if the key is present, or add a key-value pair only if the key is absent. The standard general-purpose implementation of ConcurrentMap is ConcurrentHashMap, which is a concurrent analog of HashMap.

*  `ConcurrentNavigableMap`: a subinterface of ConcurrentMap that supports approximate matches. The standard general-purpose implementation of ConcurrentNavigableMap is ConcurrentSkipListMap, which is a concurrent analog of TreeMap.


## Atomic Variables
* `java.util.concurrent.atomic` package: defines classes that support atomic operations on single variables.

* Have get and set methods that work like reads and writes on `volatile` variables.

* Avoid the liveness (deadlock, starvation, livelock) impact of unnecessary synchronization.

* [Compare and Swap (CAS)](http://en.wikipedia.org/wiki/Compare-and-swap): an atomic instruction used in multithreading to achieve synchronization.


## Concurrent Random Numbers (JDK 7)
`ThreadLocalRandom`, for applications that expect to use random numbers from multiple threads or `ForkJoinTasks`.

```java
int r = ThreadLocalRandom.current().nextInt(4, 77);
```