# Dependency Injection

Dependency injection is a software design pattern in which one or more dependencies (or services) are injected, or passed by reference, into a dependent object (or client) and are made part of the client's state.

## Element
* the implementation of a service object
* the client object depending on the service
* the interface the client uses to communicate with the service
* the injector object, which is responsible for injecting the service into the client. The injector object may also be referred to as an assembler, provider, container, factory, or spring.

## Advantages
* doesn't require any change in code behavior.
* easier to unit test in isolation using stubs or mock objects that simulate other objects not under test.
* allows a client to remove all knowledge of a concrete implementation that it needs to use.
* allows the system to be reconfigured without recompilation.
* decreases coupling between a class and its dependency.

## Disadvantages
* make code difficult to trace (read) because it separates behavior from construction.
* diminishes encapsulation by requiring users of a system to know how it works internally and not merely what task of job it performs.

## Example
### Without DI
```java
// An example without dependency injection
public class Client {
    // Internal reference to the service used by this client
    private Service service;
 
    // Constructor
    Client() {
        // Here is the main difference
        // Specify a specific implementation in the constructor instead of using dependency injection
        this.service = new ServiceExample();
    }
 
    // Method within this client that uses the services
    public String greet() {
        return "Hello " + service.getName();
    }
}
```

### DI using constructor
```java
// Constructor
Client(Service service) {
    // Save the reference to the passed-in service inside this client
    this.service = service;
}
```

### DI using setter method
```java
// Setter method
public void setService(Service service) {
    // Save the reference to the passed-in service inside this client
    this.service = service;
}
```

### DI using interface
```java
// Service setter interface.
public interface ServiceSetter {
    public void setService(Service service);
}
 
// Client class
public class Client implements ServiceSetter {
    // Internal reference to the service used by this client.
    private Service service;
 
    // Set the service that this client is to use.
    @Override
    public void setService(Service service) {
        this.service = service;
    }
}
```