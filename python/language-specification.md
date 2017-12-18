## Naming Conventions

### Variable Naming

* \_single\_leading\_underscore: weak "internal use" indicator. E.g.`from M import *`does not import objects whose name starts with an underscore.

* single\_trailing\_underscore\_: used by convention to avoid conflicts with Python keyword.

* \_\_double\_leading\_underscore: when naming a class attribute, invokes name mangling \(inside class `FooBar`, `__boo` becomes `_FooBar__boo`\). Prevent variable being confused by the same variable in subclasses.

### Decorator

A way to invoke a function which its input is a function.

#### functools.wraps

Copy the metadata \(func name, docstr, arg list\) to replace the metadata of decorator

```
from functools import wraps

def a(func):
    wraps(func)
    """This is a decorator"""
    print("Invoked as a decorator\n")
    func()

@A 
def b():
    """This is a normal function"""
    print("Normal function\n")

> b()
> Invoked as a decorator
  Normal function
  
> a.__doc__
> This is a normal function


```

#### 



