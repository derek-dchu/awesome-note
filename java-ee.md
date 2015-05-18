# Java EE

## Terms
| Term | Definition |
| -- | -- |
| Java EE (JEE) | Java Platform, Enterprise Edition |
| JCP | Java Community Process |
| JSRs | Java Specification Requests |
| POJOs | Plain Old Java Objects |
| JSP | JavaServer Page |
| [EJB](#ejb) | Enterprise JavaBeans |
| XML | Extensible Markup Language |
| SOAP | Simple Object Access Protocol |
| WSDL | Web Service Description Language |
| JAR | Java Archive file |
| WAR | Web Archive file |
| EAR | Enterprise Archive file |
| JSP | JavaServer Pages |
| JSTL | JavaServer Pages Standard Tag Library |
| JPA | Java Persistence API |
| JTA | Java Transaction API |
| JAX-RS | Java API for RESTful Web Services |
| JMS | Java Message Service |
| JACC | Java Authorization Contract for Containers |
| JASPIC | Java Authentication Service Provider Interface for Containers |
| JAF | JavaBeans Activation Framework |
| JAXP | Java API for XML Processing |
| JAXB | Java API for XML Binding |
| SAAJ | SOAP with Attachments API for Java |
| JAX-WS | Java API for XML Web Services |
| JAAS | Java Authentication and Authorization |
| JPDA | Java Platform Debugger Architecture |
| UDDI | Universal Description, Discovery and Integration |
| SOAP | Simple Object Access Protocol |
| SEI | Service Endpoint Interface |


## Web Service vs Web Application
* Web Service: produce data that can be consume by web application. Web Service can communicate with each other. It faces to server.
* Web Application: consume data from web server and interact with users. It faces to user.

### Web Service Description Language (WSDL)
Define a contract associated with a web service that used by other web services to communicate with it.

* Like an interface between different Java modules.
* Written in XML format.


## Java EE Application Model
JEE uses a distributed multitiered application model for enterprise applications. Generally, it can be divided into tiers in following list:
  1. Client-tier components run on the client machine. (Web Pages)
  2. Web-tier components run on the Java EE server. (JavaServer Page)
  3. Business-tier components run on the Java EE server. (Enterprise Beans)
  4. Enterprise information system (EIS)-tier software runs on the EIS server. (Database)


## Java EE Containers
**Containers** are the interface between a component and the low-level platform-specific functionality that supports the component.

### Container Types
* Java EE server: The runtime portion of a Java EE product, which provides EJB and web containers.
* EJB container: Manages the execution of enterprise beans.
* Web container: Manages the execution of web pages, servlets, and some EJB components.
* Application client container: Manages the execution of application client components.
* Applet container: Manages the execution of applets. Consists of a web browser and Java Plug-in running on the client together.

### Java EE APIs
### EJB
EJB is a body of code having fields and methods to implement modules of business logic.

* session bean: a transient conversation with a client. When the client finishes executing, the session bean and its data are gone.
* message-driven bean: combines features of a session bean and a message listener, allowing a business component to receive messages asynchronously.

### Servlet
A servlet class extends the capabilities of servers that host applications accessed by way of a request-resoponse programming model.

### JSP
JSP lets us put snippets of servlet code directly into a text-based document (HTML or XML).

### JSTL
JSTL encapsulates core functionality common to many JSP applications.

### JTA
JTA provides a standard interface for demarcating transactions.

### Bean Validation
The Bean Validation specification defines a metadata model and API for validating data in JavaBeans components.


## Web Application

```
            HTTP Request
Web Client --------------> HttpServlet
    ^                           |
    |  HTTP Response            |
    ---------------------- Web Components --> Database
                                |
                                |
                        JavaBeans Components --> Database
```

A web application consists of web components; static resource files, such as images; and helper classes and libraries.

### Web Application Lifecycle
1. Develop the web component code.
2. Develop the web application deployment descriptor, if necessary.
3. Compile the web application components and helper classes referenced by the components.
4. Optionally, package the application into a deployable unit.
5. Deploy the application into a web container.
6. Access a URL that references the web application.


## Web Server
A web serve uses HTTP protocal to transfer data.

* Apache Tomcat
* JBoss

## Servlet Container
Servlet container is the container for Servlets that manages the life cycle of each servlet.

## Servlet
Servlet is an interface defined in `javax.servlet` package.

Servlet can only has one instance and by default, it is lazy loaded: Server will only instantiate each servlet once and only once when there is a request for that servlet.

Using servlets allows the JVM to handle each request within a separate Java thread, and this is one of the key advantage of Servlet container.

**Note:** we can force it to be loaded when server start by setting `<load-on-startup>` to 1.

### Servlet life cycle
Servlet inferface declares three essential methods of the life cycle of a servlet

1. `void init(ServletConfig)`: it is invoked during initialization stage. It is passed an object implementing the `javax.servlet.ServletConfig` interface, which allows the servlet to access initialization parameters from the web application.
2. `void service(ServletRequest, ServletResponse)`: it is invoked upon each request after its initialization. Each request is serviced in its own separate thread. The web container calls the service() method of the servlet for every request. The service() method determines the kind of request being made and dispatches it to an appropriate method to handle the request.
3. `void destroy()`: it is invoked when the servlet object should be destroyed. It releases the resources being held.

### How web server and Servlet container process a request?
1. Web server recevies HTTP request
2. Web server forwards the request to servlet container within JVM
3. The servlet is dynamically retrieved and loaded into the address space of the container, if it is not in the container.
4. The container invokes the init() method of the servlet for initialization (invoked once when the servlet is loaded first time)
5. The container invokes the service() method of the servlet to process the HTTP request, i.e., read data in the request and formulate a response. The servlet remains in the container’s address space and can process other HTTP requests.
6. Web server return the dynamically generated results to the correct location