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
Test `addProduct` method of a Category which add a Product. The content of product is irrelevant to this test, so Mockito helps us to create a dummy object.

```java
@Test
public void testAddProductWithDummyTest() {
    Product dummy = mock(Product.class);
    Category category = new Category();
    category.addProduct(dummy);
    Assert.assertEquals(1, category.getNumberOfProducts());
}
```

### Test stub
Test stub is to return controlled values to the object being tested. These are described as indirect inputs to the test.

#### Example
Test `getHighestPrice` method of the Category which return the highest price by providing a test stub of category.

```java
public void testGetHighestPrice() throws Exception {
  Price price1 = new Price(10); 
  Price price2 = new Price(15);
  Price price3 = new Price(25);
 
  Category category = mock(Category.class);
  when(category.getPrice(any(Product.class)))
    .thenReturn(price1, price2, price3);
   
  Category category = new Category();
  Price highestPrice = category.getHighestPrice());
  
  assertEquals(price3.getPrice(), highestPrice.getPrice());
}
```