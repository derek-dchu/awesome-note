# Interfaces and Inheritance

## Upcast vs Downcast
### Upcast  
* Primitive: shorter byte to longer byte.
* Objects: upcast by "is a".

### Downcast
* Primitive: longer byte to shorter byte.  
* Objects: downcast from parent reference to child.

### Conversion:
* Primitive
  1. closest upper primitive type
  2. wrapper class  
  
* Class  
closest upper class in the inheritance tree. If there are more than one class available, there will be a compile error.