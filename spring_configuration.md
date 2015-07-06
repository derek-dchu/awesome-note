# Configuration

## Application Dependency
### XML Based
#### General Bean
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

##### Naming beans
Every bean has one or more identifiers. (`id` or `name`)
Convention: alphanumeric (myBean, fooService, etc.)

Alias:
```xml
<alias name="fromName" alias="toName"/>
```

### Java Based
#### General Bean
```java
@Bean
public BeanClass beanName() {
    return new BeanClass();
}
```

#### JNDI Datasource
```java
@Bean
public DataSource dataSource() {
    final JndiDataSourceLookup dsLookup = new JndiDataSourceLookup();
    dsLookup.setResourceRef(true);
    DataSource dataSource = dsLookup.getDataSource("jdbc/{dataSource}");
    return dataSource;
} 
```

#### Hibernate 3 AnnotationSessionFactory
```java
// Use private method for abstract bean
private AnnotationSessionFactoryBean getBaseAnnotationSessionFactoryBean() {
    AnnotationSessionFactoryBean sessionFactory = new AnnotationSessionFactoryBean();
    Properties properties = new Properties() {
        {
            setProperty("hibernate.dialect", e.getProperty("hibernate.dialect"));
            setProperty("hibernate.show_sql", e.getProperty("hibernate.show_sql"));
            setProperty("hibernate.format_sql", e.getProperty("hibernate.format_sql"));
            setProperty("hibernate.cache.use_second_level_cache", e.getProperty("hibernate.cache.use_second_level_cache"));
            setProperty("hibernate.cache.use_query_cache", e.getProperty("hibernate.cache.use_query_cache"));
            setProperty("hibernate.id.new_generator_mappings", e.getProperty("hibernate.id.new_generator_mappings"));
        }
    };
    sessionFactory.setHibernateProperties(properties);
    return sessionFactory;
}

@Autowired
@Bean
public AnnotationSessionFactoryBean sessionFactory(DataSource ds) {
    AnnotationSessionFactoryBean sessionFactory = getBaseAnnotationSessionFactoryBean();
    sessionFactory.setDataSource(ds);
    sessionFactory.setPackagesToScan("<persistence model package>");
    return sessionFactory;
}
```

#### Hibernate Transaction Manager
```java
@Autowired
@Bean
public HibernateTransactionManager transactionManager(SessionFactory sessionFactory) throws Exception {
    HibernateTransactionManager txManager = new HibernateTransactionManager();
    txManager.setSessionFactory(sessionFactory);
    return txManager;
}
```

#### Spring MVC

#### Integrate with Jersey 2
We can use Jersey-spring3 module which already handled most of the configuration for us. However, it doesn't Java config at this point, which will cause problem without any modification.

To use it in a Java config environment, we need to do following config in `WebApplicationInitializer`'s `onStartUp()`:
```java
// 1. Tell jersey-spring3 the context is already initialized
servletContext.setInitParameter("contextConfigLocation", "NOTNULL");

// 2. Add RequestContextListener manually
servletContext.addListener(new RequestContextListener());

// 3. Add Jersey Servlet
ServletRegistration.Dynamic jerseyServlet = 
    servletContext.addServlet("jersey-servlet", new ServletContainer());
jerseyServlet.setInitParameter("javax.ws.rs.Application", 
            "<resource config file from below>");
jerseyServlet.setLoadOnStartup(1);
jerseyServlet.addMapping("/rest/*");
```

    
> **Note:** Do not use `@Component` for Restful resources, because we want Jersey to manage them. Otherwise, Tomcat will complain that:
> ```
org.glassfish.jersey.server.spring.SpringComponentProvider.bind None or multiple beans found in Spring context for type class XXX, skipping the type.
```
> Jersey Servlet will not be able to mapping these classes.

#### Config Jersey 2
```java
public class RestConfig extends ResourceConfig {

    /**
     * Register JAX-RS application components.
     */
    public RestConfig() {
        register(<Restful resource>.class);

        // Reg Jackson for marshaling and parsing JSON
        register(JacksonFeature.class);
    }
}
```

Reference:
* [jersey-spring3](https://jersey.java.net/documentation/latest/spring.html)
* [jersey with json](https://jersey.java.net/documentation/latest/media.html)
