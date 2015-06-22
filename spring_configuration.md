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