## The `this` keyword

1. If the `new` keyword is used when calling the function, this inside the function is a brand new object.

2. If `apply`, `call`, or `bind` are used to call/create a function, `this` inside the function is the object that is passed in as the argument.

3. If a function is called as a method, such as `obj.method()` — `this` is the object that the function is a property of.

4. If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, this is the global object. In a browser, `this` is the `window` object. If in strict mode ('use strict'), `this` will be `undefined` instead of the global object.

5. If multiple of the above rules apply, the rule that is higher wins and will set the `this` value.

6. If the function is an ES2015 arrow function, it ignores all the rules above and receives the `this` value of its surrounding scope at the time it is created.

## How prototypal inheritance works
All JavaScript objects have a prototype property, that is a reference to another object. When a property is accessed on an object and if the property is not found on that object, the JavaScript engine looks at the object's prototype. and the prototype's prototype and so on, until it finds the property defined on one of the prototypes or until it reaches the end of the prototype chain.

## AMD vs CommonJS
CommonJS is synchronous while AMD (Asynchronous Module Definition) is obviously asynchronous. CommonJS is designed with server-side development in mind while AMD, with its support for asynchronous loading of modules, is more intended for browsers.

1. AMD syntax to be quite verbose and CommonJS is closer to the style you would write `import` statements in other languages
2. If you served all your JavaScript into one concatenated bundle file, you wouldn’t benefit from the async loading properties

## Immediately Invoked Function Expressions (IIFE)
The JavaScript parser reads function `foo(){ }()`; as function `foo(){ }` and `()`;, where the former is a function declaration and the latter (a pair of brackets) is an attempt at calling a function but there is no name specified, hence it throws Uncaught SyntaxError: Unexpected token )

Here are two ways to fix it that involves adding more brackets: `(function foo(){ })()` and `(function foo(){ }())`. These functions are not exposed in the global scope and you can even omit its name if you do not need to reference itself within the body.

## Difference between `null`, `undefined` and `undeclared`
Undeclared variables are created when you assign to a value to an identifier that is not previously created using `var`, `let` or `const`. Undeclared variables will be defined globally, outside of the current scope. In strict mode, a `ReferenceError` will be thrown when you try to assign to an `undeclared` variable. Undeclared variables are bad just like how global variables are bad. Avoid them at all cost! To check for them, wrap its usage in a try/catch block.

```js
function foo() {
  x = 1;   // Throws a ReferenceError in strict mode
}
foo()
console.log(x) // 1
```

A variable that is `undefined` is a variable that has been declared, but not assigned a value. It is of type undefined. If a function does not return any value as the result of executing it is assigned to a variable, the variable also has the value of `undefined`. To check for it, compare using the strict equality (`===`) operator or `typeof` which will give the `'undefined'` string. Note that you should not be using the abstract equality operator to check, as it will also return true if the value is `null`.

```js
var foo;
console.log(foo); // undefined
console.log(foo === undefined); // true
console.log(typeof foo === 'undefined'); // true
console.log(foo == null); // true. Wrong, don't use this to check!
function bar() {}
var baz = bar();
console.log(baz); // undefined
```

A variable that is `null` will have been explicitly assigned to the `null` value. It represents no value and is different from undefined in the sense that it has been explicitly assigned. To check for `null`, simply compare using the strict equality operator. Note that like the above, you should not be using the abstract equality operator (==) to check, as it will also return true if the value is `undefined`.

```js
var foo = null;
console.log(foo === null); // true
console.log(foo == undefined); // true. Wrong, don't use this to check!
```

## How to construct an object
`var person = new Person()` creates an instance of the Person object using the `new` operator, which inherits from `Person.prototype`. An alternative would be to use `Object.create`, such as: `Object.create(Person.prototype)`.

```js
function Person(name) {
  this.name = name;
}
var person = Person('John');
console.log(person); // undefined
console.log(person.name); // Uncaught TypeError: Cannot read property 'name' of undefined

var person = new Person('John');
console.log(person); // Person { name: "John" }
console.log(person.name); // "john"
```

## `.call` vs `.apply`
Both `.call` and `.apply` are used to invoke functions and the first parameter will be used as the value of `this` within the function. However, `.call` takes in a comma-separated arguments as the next arguments while `.apply` takes in an array of arguments as the next argument. An easy way to remember this is C for call and comma-separated and A for apply and array of arguments.

```js
function add(a, b) {
  return a + b;
}
console.log(add.call(null, 1, 2)) // 3
console.log(add.apply(null, [1, 2])) // 3
```

## `Function.prototype.bind.`
>The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

It is the most useful for binding the value of `this` in methods of classes that you want to pass into other functions. This is frequently done in React components.

## `"use strict"`
`'use strict'` is a statement used to enable strict mode to entire scripts or individual functions. Strict mode is a way to opt in to a restricted variant of JavaScript.

Advantages:

1. Makes it impossible to accidentally create global variables.
2. Makes assignments which would otherwise silently fail to throw an exception.
3. Makes attempts to delete undeletable properties throw an exception (where before the attempt would simply have no effect).
4. Requires that function parameter names be unique.
5. `this` is undefined in the global context.
6. It catches some common coding bloopers, throwing exceptions.
7. It disables features that are confusing or poorly thought out.

Disadvantages:

1. Many missing features that some developers might be used to.
2. No more access to function.caller and function.arguments.
3. Concatenation of scripts written in different strict modes might cause issues.

## What is event loop? What is the difference between call stack and task queue?
The event loop is a single-threaded loop that monitors the call stack and checks if there is any work to be done in the task queue. If the call stack is empty and there are callback functions in the task queue, a function is dequeued and pushed onto the call stack to be executed.

## Mixin
A mixin is a class or interface in which some or all of its methods and/or properties are unimplemented, requiring that another class or interface provide the missing implementations.

```js
let sayMixin = {
  say(phrase) {
    alert(phrase);
  }
};

let sayHiMixin = {
  __proto__: sayMixin, // (or we could use Object.create to set the prototype here)

  sayHi() {
    // call parent method
    super.say(`Hello ${this.name}`);
  },
  sayBye() {
    super.say(`Bye ${this.name}`);
  }
};

class User {
  constructor(name) {
    this.name = name;
  }
}

// copy the methods
Object.assign(User.prototype, sayHiMixin);

// now User can say hi
new User("Dude").sayHi(); // Hello Dude!
```

## Use event loop to avoid stackoverflow

```js
var list = readHugeList();

var nextListItem = function() {
    var item = list.pop();

    if (item) {
        // process the list item...
        setTimeout( nextListItem, 0);  // push the function to event loop, not the calls stack
    }
};
```

## What is "closure"
A closure is an inner function that has access to the variables in the outer (enclosing) function’s scope chain. The closure has access to variables in three scopes; specifically: (1) variable in its own scope, (2) variables in the enclosing function’s scope, and (3) global variables.

```js
var globalVar = "xyz";

(function outerFunc(outerArg) {
    var outerVar = 'a';
    
    (function innerFunc(innerArg) {
    var innerVar = 'b';
    
    console.log(
        "outerArg = " + outerArg + "\n" +
        "innerArg = " + innerArg + "\n" +
        "outerVar = " + outerVar + "\n" +
        "innerVar = " + innerVar + "\n" +
        "globalVar = " + globalVar);
    
    })(456);
})(123);
```

In the above example, variables from innerFunc, outerFunc, and the global namespace are all in scope in the innerFunc. The above code will therefore produce the following output:

```
outerArg = 123
innerArg = 456
outerVar = a
innerVar = b
globalVar = xyz
```

## How do you clone an object?
```js
var obj = {a: 1, b: 2}
var objclone = Object.assign({}, obj);
```

Now the value of objclone is {a: 1 ,b: 2} but points to a different object than obj.

Note the potential pitfall, though: Object.clone() will just do a shallow copy, not a deep copy. This means that nested objects aren’t copied. They still refer to the same nested objects as the original:

```js
let obj = {
    a: 1,
    b: 2,
    c: {
        age: 30
    }
};

var objclone = Object.assign({},obj);
console.log('objclone: ', objclone);

obj.c.age = 45;
console.log('After Change - obj: ', obj);           // 45 - This also changes
console.log('After Change - objclone: ', objclone); // 45
```