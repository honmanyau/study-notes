# JavaScript

## Table of Contents

* [JavaScript](#javascript)
  * [Terminology](#terminology)
  * [Patterns](#patterns)
  * [Questions](#questions)
  * [Resources](#resources)

## Terminology

### Boxing

The behaviour where a primitive is automatically "boxed"/"wrapped" with the appropriate native/built-in function so that the functionalities required by the author is made available. For example:

```JavaScript
const str = 'nyanpasu'; // Primitive (typeof str returns "string")

str.length; // Boxing occurs here—effectively (new String(str)).length
```

In addition:

> Browsers long ago performance-optimized the common cases like .length, which means your program will actually go slower if you try to "preoptimize" by directly using the object form (which isn't on the optimized path).

See also [unboxing](#unboxing).

Reference: [YDKJS, Types & Grammar, Chapter 3](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch3.md)

### Call Stack

A interpreter's mechanism for keeping track its location in code and prioritising the order that functions should be called.

For example, when the interpreter encounters a function call, say `fn()`, it is added to the call stack and executed. Once every line inside `fn()` has been executed, the interpreter goes back to where `fn()` was initially invoked, continues to execute the rest of the code, and removes `fn()` from the call stack.

The call stack is a LIFO data structure.

Reference: [MDN Web Docs, Call Stack](https://developer.mozilla.org/en-US/docs/Glossary/Call_Stack)

### Closure

> Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

Reference: [YDKJS, Scope & Closures, Chapter 5](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md)

### Coercion

The definition varies but in the context of JavaScript, and according to common "understanding", coercion usually refers to the process in which a value is converted from one type to another, whether explicitly or implicitly, in a dynamically-typed language. For example:

```JavaScript
const num = 42;

String(num); // "42", explicit
num + ''; // "42", implicit
```

In JavaScript, coercions **always** result in a scalar primitive.

Continuing on with the distinction made above, the corresponding, explicit conversion in a statically-typed language is called type casting.

Reference: [YDKJS, Types & Grammar, Chapter 4](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch4.md)

### Falsy

Values that are coerced to `false` when evaluated as a condition, in comparisons, or in a conditional statement. In standard JavaScript, these values are:

* `""` or `''`
* `0`, `-0` or `NaN`
* `null`
* `undefined`
* `false`

In addition, where browser APIs are involved:

* `document.all` (yes, this is a falsy **object** for which `typeof` returns `undefined`...)

Reference: [YDKJS, Up & Going, Chapter 2](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch2.md), [YDKJS, Types & Grammar, Chapter 4](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch4.md)

### Hoisting

The behaviour where a variable or function declared is is accessible inside the entire enclosing scope. Variables declared with a `variable` statement using the `var` keyword and functions declaration using the `function` keyword are hoisted. In JavaScript, functions are hoisted before variables.

Reference: [YDKJS, Scope & Closures, Chapter 4](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch4.md)

### Host Environment

A container that primarily converts source code into executable code with an engine and provides a set of tools for interacting with the host application. In terms of JavaScript, the host environment could be a web browser like Firefox or a Node.js environment, which are built on different engines.

Just as host environments may be built on different engines, the tools that are globally available (through APIs) are also different and are not part of standard JavaScript/ECMAScript. For example, `console.log()` is not part of standard JavaScript and is provided by the host environment.

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

### Reference-Copy

When a copy of the reference to a value is created before it is assigned or passed. In JavaScript, this happens with the composite data types such as objects, arrays and functions. For example:

```JavaScript
let objA = { a: 1, b: 2 };
let objB = objA;

objB.c = 3;

console.log(objA); // Object { a: 1, b: 2, c: 3 }
console.log(objB); // Object { a: 1, b: 2, c: 3 }
```

See also [value-copy](#value-copy).

Reference: [YDKJS, Types & Grammar, Chapter 2](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch2.md)

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

### Unboxing

The value stored in a "boxed" object can be accessed using the `valueOf()` function or by implicit means. For example:

```JavaScript
let str = new String('nyanpasu');

typeof str.valueOf(); // "string"
typeof (str + '') // "string"
```

See also [boxing](#boxing).

Reference: [YDKJS, Types & Grammar, Chapter 3](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch3.md)

### Value-Copy

When a copy of a value is created before it is assigned or passed. In JavaScript, this happens with the primitive data types. For example:

```JavaScript
let a = 'nyan';
let b = a;

b = 'pasu';

console.log(a); // "nyan"
console.log(b); // "pasu"

// Assignment to 'b' does not affect the value of 'a' because 'nyan' is a
// string primitive and a copy of it is created with the assignment let b = a;
```

See also [reference-copy](#referece-copy).

Reference: [YDKJS, Types & Grammar, Chapter 2](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch2.md)

## Patterns

### Cooperativity

An approach that employs asynchrony to break a task into smaller chunks such that the thread can be freed up temporarily for other tasks queued in the call stack. For example:

```JavaScript
const collection = [];

function processData(data) {
  const chunk = data.splice(0, 1000).map((user) => {
    const { firstName, lastName } = user;

    return `${firstName} ${lastName}`;
  });

  if (data.length) {
    setTimeout(() => {
      processData(data);
    }, 0);
  }
}

fetch('https://nyanpasu.com/api/endpoint1')
  .then((response) => response.json())
  .then((data) => processData);

fetch('https://nyanpasu.com/api/endpoint2')
  .then((response) => response.json())
  .then((data) => processData);
```

Reference: [YDKJS, Async and Performance, Chapter 1](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch1.md)

### Immediately Invoked Function Expression (IIFE)

A function that is immediately executed after it is created. This pattern is used to avoid scope-based variable conflicts as variables declared inside the IIFE are only scoped to that function.

### Module

A common pattern that utilises the concept of closure for hiding private implementations and exposing public methods via an AIP. For example:

```JavaScript
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

### Do you know of a use for scope in terms of garbage collection?

`Placeholder`

### Given an object, `a`, how does one create another object, `b`, that is prototype-linked to `a` using 1. `Object.create()` and 2. `Object.setPrototypeOf()`?

`Placeholder`

### Given that JavaScript is single-threaded, how does asynchrony actually work in JavaScript? (Sort of a trick question)

Asynchrony in JavaScript does not refer to multiple processes or threads running at the same time (as in parallelism).

It simply refer to the behaviour where code execution is "deferred" until a response is received from the host environment. When the response is picked up by the host environment, it is enqueued by the event loop as a message to the... well, queue, which contains also the function to be executed (callback function). When the call stack is empty, the event loop will dequeue its messages and call the callback functions, one by one, in a FIFO manner.

In essence, this is simply how the event loop and call stack works together to begin with, and it's simply a parallax error on the part of the observer, presumably caused by concurrency, to think that things are happening in parallel.

Reference: [YDKJS, Async & Performance, Chapter 1](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch1.md), [MDN Web Docs, Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop), [MDN Web Docs, Call Stack](https://developer.mozilla.org/en-US/docs/Glossary/Call_Stack)

### Given the assignments `let a = new String('nyanpasu')` and `"let b = nyanpasu"`, what is the difference between `a` and `b`? (Potential follow-up questions: what do `a.length` and `b.length` return? In addition, why does `b.length` returns what it returns?)

`Placeholder`

### Given three variables `a`, `b` and `c`, and that they are assigned, **in no particular order** the following expressions `new String('nyanpasu')`, `new Array('nyanpasu')`, `{ 'nyan': 'pasu' }`; what would the `typeof` operator return for each of the variables? How would you determine whether the given variables is a string, an array, or an object?

`typeof` will return `"object"` in all cases. One way to quickly inspect the internal classification of the object is to use `Object.prototype.toString.call()`.

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

### What is the difference between `new Date()` and `Date()`?

They both return the current time; however, `new Date()` returns a `Date` object/data structure and `Date()` returns a string primitive in a non-standardised format.

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

### What is returned by the statement `parseInt( 1/0, 19 )` and why?

`Placeholder`

### Where does the prototype chain end? (Potential follow-up question: what can be found there?)

`Object.prototype`.

### Why is the statement `var a = b = 42;` potentially a bad idea?

`Placeholder`

## Resources

* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
* [MDN Web Docs, JavaScript Expression and Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)
* [MDN Web Docs, JavaScript Data Types and Data Structures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)
* [MDN Web Docs, Standard Built-in Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
* [MDN Web Docs, Glossary](https://developer.mozilla.org/en-US/docs/Glossary)
* [MDN Web Docs, Operator Precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)
