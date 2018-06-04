# JavaScript

## Table of Contents

* [JavaScript](#javascript)
  * [Terminology](#terminology)
  * [Patterns](#patterns)
  * [Questions](#questions)
  * [Resources](#resources)

## Terminology

### Closure

> Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

Reference: [YDKJS, Scope & Closures, Chapter 5](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md)

### Falsy

Values that are coerced to `false` when evaluated as a condition, in comparisons, or in a conditional statement:

* `""` or `''`
* `0`, `-0` or `NaN`
* `null`
* `undefined`
* `false`

Reference: [YDKJS, Up & Going, Chapter 2](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch2.md)

### Hoisting

The behaviour where a variable or function declared is is accessible inside the entire enclosing scope. Variables declared with a `variable` statement using the `var` keyword and functions declaration using the `function` keyword are hoisted. In JavaScript, functions are hoisted before variables.

Reference: [YDKJS, Scope & Closures, Chapter 4](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch4.md)

### Key Access

Accessing the value of a property in an object using the `[]` syntax; for example, `obj['property']`. The property name accessed in this manner is a UTF-8/unicode-compatible string.

Reference: [YDKJS, `this` & Object Prototypes](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch3.md)

### Polyfilling

The act of replicating new features in the language using code that is compatible with older browsers. Notable example: https://github.com/es-shims/es6-shim.

### Property Access

Accessing the value of a property in an object using the `.` syntax; for example, `obj.property`. The property name accessed in this manner **must** be `Identifier`-compatible.

Reference: [YDKJS, `this` & Object Prototypes, Chapter 3](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch3.md)

### Shadowing

Assigning value to a property that is to a given object, where the same property can be found higher up in its prototype chain.

> Usually, shadowing is more complicated and nuanced than it's worth, so you should try to avoid it if possible.

Reference: [YDKJS, `this` & Object Prototypes, Chapter 5](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch5.md)

### Property Descriptors

A set of properties that describes how a given property in an object behaves; these properties include `value`, `writable` (whether or not a new value can be assigned to that property), `configurable` (whether or not the descriptors of a given property an be modified—this is a one way change!) and `enumerable` (whether or not a property is processed in enumerations, such as in a `for` loop).

Reference: [YDKJS, `this` & Object Prototypes, Chapter 3](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch3.md)

### `this`

The ultimate pun creator.

Probably the most misunderstood concept in JavaScript, so much so that that `this` is probably won't come up in your next technical interview.

Jokes aside, how `this` works is very nicely detailed in this & Object Prototypes of the YDKJS series. In essence, and in order of precedence:

1. `new` binding—where `this` is the newly created object.
2. Explicit binding with `call` or `apply`, or hard binding with `bind`—where `this` is the specified object
3. Implicit binding—where `this` is the context object
4. Default binding—when none of the above applies, where `this` is `undefined` under `strict mode` and the `global` object otherwise

Reference: [YDKJS, `this` and Object Prototypes, Chapter 1](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md)

### Transpiling

The act of converting code writing with a newer syntax into code that is compatible with older browsers that do not support the newer syntax. Notable example: https://babeljs.io.

### Truthy

Anything that is not [falsey](#falsy).

### Types

As of ES6, there are six primitive data types in JavaScript: boolean, null, undefined, number, string and symbol.

The remaining type, object, is a composite data type. It is worth noting that arrays and functions are also objects in JavaScript.

Reference: [MDN Web Docs, JavaScript Data Types and Data Structures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)

## Patterns

### Immediately Invoked Function Expression (IIFE)

A function that is immediately executed after it is created. This pattern is used to avoid scope-based variable conflicts as variables declared inside the IIFE are only scoped to that function.

### Module

A common pattern that utilises the concept of closure for hiding private implementations and exposing public methods via an AIP. For example:

```javascript
function Cat() {
  let privateFirstName, privateLastName, privateFullName;

  function init(firstName, lastName) {
    privateFirstName = firstName;
    privateLastName = lastName;
    privateFullName = `${firstName} ${lastName}`;
  }

  function meow() {
    console.log(`Meow, ${fullName}!`);
  }

  const publicAPI = {
    init: init,
    meow: meow
  }

  return publicAPI;
}

const cat = Cat();

cat.init('Nyan', 'Pasu');
cat.meow(); // "Meow, Nyan Pasu!"
```

Reference: [YDKJS, Up & Going, Chapter 2](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch2.md) and [YDKJS, Scope & Closures, Chapter 5](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md).

## Questions

### Can you tell me anything about `Object.create(null)`?

`Placeholder`

### Comment on the use of `var` vs. `let` in terms of `for` loops.

`Placeholder`

### Describe an example, such as a common pattern, that demonstrates the concept of closure.

`Placeholder`

### Describe a common use of `Function.prototype.apply`/`Function.prototype.bind` other than explicit/hard binding. (Potentially follow up: how would you change your code to insulate any potential side effects on the `global` object?)

`Placeholder`

### Describe how `in`/`for...in` works. (Potential follow-up questions: do you have anything to add if given that we are enumerating an object created with `Object.create()`?)

`Placeholder`

### Describes some methods for copying objects and discuss the differences between them.

`Placeholder`

### Do you know of a use for scope in garbage collection?

`Placeholder`

### Given an object, `a`, how does one create another object, `b`, that is prototypically delegated to `a` using 1. `Object.create()` and 2. `Object.setPrototypeOf()`?

`Placeholder`

### How does one prevent further properties from being added to an object? (Potential follow ups: 1. How would one achieve the same if it is an array? 2. What are the differences between the functions mentioned?)

`Object.preventExtensions()`, `Object.seal()`, `Object.freeze()`.

### How does one create an object using the object literal syntax with computed properties?

`Placeholder`

### Implicit in JavaScript coercion is evil.

No, not necessarily; and that statement suggests a lack of understanding of the rules behind implicit coercion.

### Is JavaScript a compiled language or an interpreted language?

> The JavaScript engine actually compiles the program on the fly and then immediately runs the compiled code.

Reference: [YDKJS, Up & Going, Chapter 1](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch1.md)

### What are some potential gotchas that you may run into When updating older code to use the newer `let` and `const` variable declaration keywords?

`Placeholder`

### What are the differences between `var`, `let`, and `const`?

`Placeholder. Discuss function vs. block scope; hoisting; reasssignment; what happens when and object or array is declared with const; merits and common pitfalls (where applicable) of each of them.`

### What does the `new` operator do?

> ... sort of hijacks any normal function and calls it in a fashion that constructs an object, in addition to whatever else it was going to do.

And is one of the common causes of confusion that classes actually exist in JavaScript.

Reference: [YDKJS, `this` and Object Prototypes, Chapter 5](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch5.md)

### What happens when one attempts to assign a value to a property that is not found on a given object but is present higher up in its prototype chain? (Potential follow-up question: what if the value of that property is a setter?)

`Placeholder`

### What is the difference between `'p' in obj` and `obj.hasOwnproperty('p')`?

The `in` keyword traverses the prototype chain of an object and `hasOwnproperty()` does not.

Reference: [YDKJS, `this` & Object Prototypes, Chapter 3](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch3.md)

### What is the only value in JavaScript that is not equal to itself?

`NaN`.

### Where does the prototype chain end? (Potential follow-up question: what can be found there?)

`Object.prototype`.

## Resources

* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
* [MDN Web Docs, JavaScript Expression and Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)
* [MDN Web Docs, JavaScript Data Types and Data Structures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)
