# JavaScript

## Table of Contents

* [JavaScript](#javascript)
  * [Terminology](#terminology)
  * [Patterns](#patterns)
  * [Questions](#questions)
  * [Resources](#resources)

## Terminology

### Falsy

Values that are coerced to `false` when evaluated as a condition, in comparisons, or in a conditional statement:

* `""` or `''`
* `0`, `-0` or `NaN`
* `null`
* `undefined`
* `false`

Reference [YDKJS](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch2.md)

### Hoisting

The behaviour where a variable or function declared is is accessible inside the entire enclosing scope. Variables declared with a `variable` statement using the `var` keyword and functions declaration using the `function` keyword are hoisted.

### Polyfilling

The act of replicating new features in the language using code that is compatible with older browsers. Notable example: https://github.com/es-shims/es6-shim.

### Transpiling

The act of converting code writing with a newer syntax into code that is compatible with older browsers that do not support the newer syntax. Notable example: https://babeljs.io.

### Truthy

Anything that is not [falsey](#falsy).

### Types

As of ES6, there are six primitive data types in JavaScript: boolean, null, undefined, number, string and symbol.

The remaining type, object, is a composite data type. It is worth noting that arrays and functions are also objects in JavaScript.

Reference [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)

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

Reference [YDKJS](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch2.md).

## Questions

### Implicit in JavaScript coercion is evil.

No, not necessarily; and that statement suggests a lack of understanding of the rules behind implicit coercion.

### Is JavaScript a compiled language or an interpreted language?

> The JavaScript engine actually compiles the program on the fly and then immediately runs the compiled code.

Reference [YDKJS](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch1.md).

### What are the differences between var, let, and const?

`Placeholder`

### What is the only value in JavaScript that is not equal to itself?

`NaN`.

## Resources

* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
* [JavaScript Expression and Operators, MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)
* [JavaScript Data Types and Data Structures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)
