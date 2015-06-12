# Unit Test

## Test Double
```
            Test Double
            
    Dummy   Test    Test    Mock    Fack
    Object  Stub    Spy     Object  Object
```

Reference: [Test Double](http://xunitpatterns.com/Test%20Double.html)

### Dummy Object
An object that has no implementation which is used purely to populate arguments of method calls which are irrelevant to the test.

#### Example
Test add method of a Category which add a Product.

```java
@Test
public void addProductWithDummyTest() {
    Product dummy = mock(Product.class);
    Category category = new Category();
    category.addProduct(dummy);
    Assert.assertEquals(1, category.getNumberOfProducts());
}
```