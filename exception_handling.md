# Exception Handling
## What is an Exception
An exception is an event that occurs during the execution of a program that disrupts the normal flow of instructions.


## The `Throwable` Class
It is a concrete class. 

```
	Throwable
	/		\
Error		Exception (checked)
(unchecked)				|
			RuntionException (unchecked)
```


## checked vs unchecked
### Checked exception 
It is a compile time exception, the compiler will force you to handle it or a compile-time error is thrown.

### Unchecked exception
It is a runtime exception, it happens during runtime. we may or may not handle it.

#### Error
Error are exceptional conditions that are external to the application, and that the application usually cannot anticipate or recover from.

#### Runtime Exception
Runtime exception are exceptional conditions that are internal to the application, and that the application usually cannot anticipate or recover from.


## How to `throw` an exception
Throwing an exception means an method creating an exception object and handing it to the runtime system.

```java
throw someThrowableObject;
```


## The Catch or Specify Requirement
When encounter an exception, we can
  1. catch and handle it
  2. specify that it will be throw again.

### Catching and Handling
After an method throws an exception, the runtime system will search the call stack in reverse order. When an appropriate *exception handler* is found, the runtime system passes the exception to the handler which is said to `catch` the exception.

If the runtime system can not find an handler, then it will terminate.

#### try ... catch ... finally
```java
try {
	
} catch (ExceptionA ea) {
	
} catch (ExceptionB eb) {
	
} finally {
	
}
```
* subclass exception will put on top of superclass.
* never put `return` in finally, because it will hide all other `return`s.
* finally block will execute after `return` in other clauses.
* Java 7, a single `catch` block can handle more than on type of exception, `catch (ExceptionA | ExceptionB ex)`, and `ex` is `final`.

#### try-with-resources
A *resource* is an object that must be closed after the program is finished with it. try-with-resources statement ensures that each resource is closed at the end of the statement.

```java
try ( object of class that implements `AutoCloseable`  ) { }
```

## How to Create Exception Classes
* expected to recover -> checked exception
* cannot do anything to recover -> unchecked exception

