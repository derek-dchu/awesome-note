# Configuration

## Application Dependency
### Java Configuration
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