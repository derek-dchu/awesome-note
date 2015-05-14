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

Thread.sleep causes the current thread to suspend execution for a specified period. We cannot assume that invoking sleep will suspend the thread for precisely the time period specified.

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
* If an object is `volatile`, then all thread can read it, but only one thread can write.
* If a thread modify a `volatile` object, this modification will be seen by all other threads immediately.


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

Benefits: immutable object is thread-safe and commonly used in multi-threading environment.

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


## High Level Concurrency Objects (Java 5+)
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





