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
Test add method of a Category which add a Product. The content of product is irrelevant to this test, so Mockito helps us to create a dummy object.

```java
@Test
public void addProductWithDummyTest() {
    Product dummy = mock(Product.class);
    Category category = new Category();
    category.addProduct(dummy);
    Assert.assertEquals(1, category.getNumberOfProducts());
}
```