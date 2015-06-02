# Spring
## Modules
```
Data Access/Integration	 				Web
(JDBC, ORM, OXM, JMS)	 (WebSocket, Servlet, Web, Portlet)
	Transactions					|
			\				 		/
AOP	Aspects	Instrumentation Messaging
				|
			Core Container
				|
			  test
```

### Core Container
1. spring-core
2. spring-beans
3. spring-context
4. spring-context-support
5. spring-expression.

### AOP and Instrumentation
1. spring-aop
2. spring-aspects
3. spring-instrument
4. spring-instrument-tomcat

### Data Access/Integration
1. spring-jdbc
2. spring-tx
3. spring-orm
4. spring-oxm
5. spring-jms

### Web
1. spring-web
2. spring-webmvc
3. spring-websocket
4. spring-webmvc-portlet

### Test
1. spring-test

* Spring Security
* Spring Integration (Struts2, REST, SOAP)
* Spring Front-end

* Each of module has their own jarfiles

**note:** Spring 2.x comes with `lib` folder contains (dependent library jarfiles), Spring 3.x ship without `lib` folder.

## (#8) Configuration File
### Header


## `log4j` module
Only one singleton class: `Logger`.

```java
Logger log = Logger.getLogger();

log.append()
log.debug()
log.warm()
log.info()
```

## Inversion of Control (IoC) ~ Dependency Injection (DI)
Objects define their dependencies only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned form a factory method. The container then injects those dependencies when it creates the bean.

* keywords:
  1. Factory Pattern: id always reference an object
  2. Reflection: inject values into fields
  3. Decoupling: decouple Data & Logic, actual value & bean structure.

### IoC Container
`org.springframework.beans` and `org.springframework.context` are the basis for IoC container.

* `BeanFactory`: deprecated
* `org.springframework.context.ApplicationContext` represents the IoC container and is responsible for instantiating, configuring and assembling the objects.

```
-- POJOs -------------> IoC Container -> Fully configured system					
-- config metadata -/
```

#### Configuration
1. XML-based

```xml
<?xml ... ?>
<beans xmlns=""
	xmlns:xsi=""
	xsi:schemaLocation="">

	<bean id="..." class="...">
		<!-- collaborators and configuration for this bean go here -->
	</bean>

	<!-- more bean definition -->
<beans>
```

2. Annotation-based
3. Java-based

##### Naming beans
Every bean has one or more identifiers. (`id` or `name`)
Convention: alphanumeric (myBean, fooService, etc.)

Alias:
```xml
<alias name="fromName" alias="toName"/>
```

#### Instantiating a container
```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml"); // src is the root folder

ApplicationContext context = new FileSystemXmlApplicationContext("file.xml"); // project folder is the root folder
```

Configuration metadata can be imported:
File path is related to the definition file doing the importing.

```xml
<beans>
	<import resource="<file xml>"/>
</beans>
```

#### Using the container
```java
Bean bean = context.getBean("bean name", bean.class);
```

**note:** Ideally our application code should never use `ApplicationContext` to retrieve beans.


### Dependency Injection
#### Setter Injection	(use setter to inject fields)
```xml
<bean id="ID" class="package.class_name>" scope="">
	<property name="field_name" value="" />
	<property name="field_name">
		<value>??</value>
	</property>
	<property name="field_name" ref="bean_id" />
</bean>
```

**note:** the class may not have a field that matches exactly the property name, but it must has a setter matching that property name (`public void set<field>`).

#### Constructor Injection
```xml
<bean id="ID" class="package.class_name>" scope="">
	<constructor-arg type="" value=""/>
	<constructor-arg>
		<ref bean="bean_id" />
	</constructor-arg>
	<constructor-arg index="0" value=""/>

	<!-- use with @ConstructorProperties({name1, name2}) for the constructor -->
	<constructor-arg name="field_name" value=""/>
</bean>
```

* Difference between setter injection and constructor injection:
  1. CI is good for mandatory dependencies and SI for optional dependencies.
  2. Partial dependency: setter injection only
  3. If both injection have be defined, IoC container will use the setter injection.
  4. Setter injection can make update to beans without create a new one.
* What is method injection?

#### Lazy-initialized beans
Pre-instantiation is desirable, because errors in the configuration or surrounding environment are discovered immediately. However, we can tell the IoC container to create a bean instance when it is first requested(lazy-initialized)

```
BeanFactory	(all lazy-initialized) (mobile application)
	^
	|
ApplicationContext (not lazy-initialized) : all beans are created after `new FileSystemXmlApplicationContet("<config.xml>")` (enterprise application)
```

* We control lazy loading feature for both of them by setting `<bean lazy-init="default|true|false"/>`.
* Container level: `<beans default-lazy-init="true"/>`

#### Autowiring
Automatically injection other beans inside current bean implicitly (usually `ref` is better)
  1. no: must be defined via a `ref` element.
  2. byName: use field name to match other beans
  3. byType: user field type to match other beans (but this cannot handle the situation that different beans have same type or subtype)
  4. constructor: analogous to byType, but applies to constructor arguments.
  5. autodetect (spring 2.x, deprecated)

* excluding a bean from autowiring: `<bean autowire-candidate="false"/>`.
* container level limitation: `<beans default-autowire-candidate="a_pattern_of_bean_names"/>.

#### Method Injection
Inject a method: dynamically override the implementation of a method.

* the method to be injected requires a signature of the following form:
  `<public|protected> [abstract] <return-type> theMethodName(no-arguments);`s

#### Singleton Injection
By default Spring uses reflection to bypass private constructor and instantiate an new object. Therefore, a singleton in Spring will return different objects under scopes except singleton.

* We HAVE to specify `factory-method="getInstance"` to force Spring get a singleton object in all scopes.
* Without `factory-method`, even in singleton scope, we will have two objects from a singleton. One is from reflection API, the other one is from `getInstance()`.


### Scope
1. singleton (default): single instance per container (each definition).
2. prototype (shallow copy): any number of instances.  

Blow scopes are only available for a web-aware Spring `ApplicationContext` implementation.
3. request: each HTTP request has its own instance of a bean.
4. session: each HTTP `Session` has its own instance of a bean.
5. global session: each global HTTP `Session` has its own instance of a bean. Typically only valid when used in a portlet context.
6. application: each `ServletContext` has its own instance of a bean.

* use the prototype for all stateful beans and the singleton for stateless beans.
* Spring does not manage the destruction lifecycle of a prototype bean.
* request, session, global session, application scopes work with Spring Web MVC `DispatcherServlet` or `DispatcherPortlet` without special setup.
* For request, session, global session scoped beans, we must inject an AOP proxy in place of them.


## AOP: Aspect Oriented Programming
* Purpose of AOP: Build dependency among separate classes
* Key concepts:
  1. Advice (what)
  2. Pointcut (where)
  3. Advisor = Advice + Pointcut
* Proxy Pattern


## Cache (Spring 3.x)


## Spring DAO
CMP: Container-Managed Persistence
BMP: Bean-Managed Persistence


## Transaction Propagation
```
BeginTx
	BeginTx
		...
	Commit
	...
	BeginTx
	Commit
Commit
```

* Reqiured:
```
BT 						BT
	#do1 					#do1
	BT 						#do2
		#do2	=> 			#do3
	CT 					CT
	#do3
CT
```
Any inner transaction fails will rollback the outer transaction (Default behavior of propagation)



## MVC
1. method driven
method return ModelAndView -> add prefix, sufferfix to generate the view.

* The method can return
  1. Model(data) and View(page) (return a `ModelAndView` object)
  2. View only (return a String as the view name)
  3. Model only (use `@ResponseBody`)


```
			@Component
		/		|		\
@Controller  @Service  @Repository (DAO)
```


## Security
* Proxy Pattern
* Business Delegate
* filter
* listener & context-param
```
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>
		/WEB-INF/spring-security.xml
	</param-value>
</context-param>
```
  * By default, it uses `applicationContext.xml`
* config file
  1. intercept-url (pattern to apply spring security)


### How to initialize fields without instantiate the class (e.g. for controllers).

```
Initialize a collection.

<util:list id="<bean>">
	<bean class="<class>">
		<property name="" value="" />
	</bean>
<util:list>
```


## Quartz
Scheduling System.




