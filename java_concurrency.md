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
It is like a stop sign. Push the current thread into READY status. When a thread calls `t.join()`, it will wait until thread `t` is finished.

### `yield` method
It is like a yield sign. Any thread calls `yield()` will make it back to READY status and wait for OS to pick it up.

* `yield()` is a static method.

### `sleep` method
`static void sleep(long millis)`,
`static void sleep(long millis, int nanos)`: millisecond + nanosecond.

Thread.sleep causes the current thread to suspend execution for a specified period. The sleep period can be terminated by interrupts. We cannot assume that invoking sleep will suspend the thread for precisely the time period specified.

### `wait` method
Causes the current thread to wait until another thread invokes the notify() method or the notifyAll() method for this object. In other words, this method behaves exactly as if it simply performs the call wait(0). So `wait()` has no control.

* If we use `wait()` without synchronized, it will throw `IllegalMonitorStateException` - the current thread is not the owner of the object's monitor.

### `notify` method
Wake up the thread that is waiting on this object's monitor.

* If we use `notify()` without synchronized, it will throw `IllealMonitorStateException`.

### `notifyAll`
Wake up all threads that are waiting on this object's monitor. A thread waits on an object's monitor by calling one of the wait methods.


## Daemon Thread
Daemon Thread is a special thread that keeps running until all other threads stop and is forced to stop by OS.

* Garbage collection is a daemon thread.

### Convert regular thread to daemon thread
```java
t.setDaemon(true);
t.start();
```

## Synchronized
```java
synchronized void foo() { ... }
synchronized (obj) { ... }
```

### Lock <=> Monitor
* methods don't have lock
* to call a synchronized static method, the caller requires the lock of the class which contains that method. (class level lock)
* to call a synchronized non-static method, the caller requires the lock of the object which contains that method. (object level lock)
* a class level lock can be converted to object level lock

```java
class A {
	static void foo2() {
		synchronized(A.class) {
			/* ... */
		}
	}
}
```


## Thread-safe
1. the class is thread-safe: only one thread can access that class at a time.
2. the program is thread-safe: return consistence output.


## Mutex


## Semaphore


## How to prevent dead lock
1. Use `Lock` class (safe lock)


## `volatile` keyword
* If an object is `volatile`, then all thread can read it, but only one thread can write.
* If a thread modify a `volatile` object, this modification will be seen by all other threads immediately.


## How to stop a thread?
Doesn't support. Because stop a thread during running may lead to memory leak.


## `interrupt` method
It throws an exception.


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