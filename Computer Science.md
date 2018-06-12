# Computer Science

## Table of Contents

* [Computer Science](#computer-science)
  * [Terminology](#terminology)

## Science Terminology

### Data Structure

> A collection of data values, the relationships among them, and the functions or operations that can be applied to the data.

Reference: [Wikipedia, Data Structure](https://en.wikipedia.org/wiki/Data_structure)

### Endianness/Byte-order

The order in which bytes that makes up numbers are stored. Little-endian refers to when bytes of a number are stored from the least significant to the most significant byte, and big-endian refers to the opposite order.

For example, the number `1337` is stored as 16-bit number, 2 bytes are required and the number is stored as `0x39` `0x05` according to the little-endian system, and `0x05` `0x39` according to the big-endian system.

Reference: [YDKJS, ES6 & Beyond, Chapter 5](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20%26%20beyond/ch5.md), [MDN Web Docs, Endianness](https://developer.mozilla.org/en-US/docs/Glossary/Endianness)

### Expression

> An expression is any reference to a variable or value, or a set of variable(s) and value(s) combined with operators.

Expressions are most often combined to make [statements](#statement) that perform certain tasks.

Some examples of expressions include, but not limited to, literal value expression (`42`); literal string expression (`"nyanpasu"`); arithmetic expression (`n * 42`); variable expression (`nya`); and assignment expression (`nya = n * 42`). It is worth noting that a function call (`console.log()`) is also an expression.

Reference: [YDKJS, Up & Going, Chapter 1](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch1.md).

### Operators

Operators are used to perform actions on values and variables.

### Primitive Data Type

A data type that cannot be decomposed to smaller components.

### Process

A process is an image of the executable machine code of a program with resources that includes, but not limited to, memory and system specific descriptors and attributes required, either by the system and/or the program, to carry out certain tasks.

Reference: [Process (Computing)](https://en.wikipedia.org/wiki/Process_(computing))

### Scope

The result of a process in which, for a given location in some code, a set of rules for storing and retrieving variables is determined. See also [this figure](https://raw.githubusercontent.com/getify/You-Dont-Know-JS/master/scope%20%26%20closures/fig2.png) in YDKJS.

Reference: [YDKJS, Scope & Closures, Chapter 2](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/fig2.png)

### Statement

> A group of words, numbers, and operators that performs a specific task is a
statement."

Example:

```
nyan = pasu * 2;
```

Reference: [YDKJS, Up & Going, Chapter 1](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch1.md).

### Static Typing

A language implementation that requires variables declared under this scheme to hold a specific type of value.

### Thread

A subset/component of a process and is the "is the smallest sequence of programmed instructions that can be managed independently by a scheduler".

Where multiple threads are concerned, they share the same memory address space (stack, heap, data, code/text) but each of them has its own call stack.

Reference: [Wikipedia, Thread (Computing)](https://en.wikipedia.org/wiki/Thread_%28computing%29#Threads_vs._processes)

### Weak/Dynamic Typing

A language implementation that allows variables declared under this scheme to hold values of any type.

## Questions

### What is scope? Give an example to illustrate how you would utilise the concept of scope when designing software.

`Placeholder`

### What are the relationship and difference between a process and a thread? (Potential follow-up question: what do and do not multiple threads share?)

`Placeholder`
