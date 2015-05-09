# Java EE

## Terms
* Java EE (JEE): Java Platform, Enterprise Edition
* JCP: Java Community Process
* JSRs: Java Specification Requests
* POJOs: Plain Old Java Objects
* JSP: JavaServer Page


## Java EE Application Model
JEE uses a distributed multitiered application model for enterprise applications. Generally, it can be divided into tiers in following list:
  1. Client-tier components run on the client machine. (Web Pages)
  2. Web-tier components run on the Java EE server. (JavaServer Page)
  3. Business-tier components run on the Java EE server. (Enterprise Beans)
  4. Enterprise information system (EIS)-tier software runs on the EIS server. (Database)


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