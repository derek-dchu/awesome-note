# Test

## Unit Test
Dependency: org.springframework.spring-test

### JUnit
#### Generate Test Case
```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {config.class,...,},
        loader = AnnotationConfigContextLoader.class)
@ActiveProfiles("dev")
@Transactional
@TransactionConfiguration(transactionManager="txMgrBeanName", defaultRollback=false)
public class DemoTestCase {
    ...
}
```

* Use `AnnotationConfigContextLoader.class` in `ContextConfiguration` for Java class config.
* Use `ActiveProfiles("profileName")` for loading config from different profiles.
