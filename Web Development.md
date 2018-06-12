# Web Develoopment

## Table of Contents

* [Web Development](#web-development)
  * [Terminology](#terminology)
  * [Patterns](#patterns)
  * [Questions](#questions)
  * [Resources](#resources)

## Terminology

### Data URI

> ... a way to include data in-line in web pages as if they were external resources.

Caveats:

* The size of a data URI can be larger than that the resource(s) it is derived from.
* Potentially difficult to maintain
* Relies on caching

References: [Wikipedia, Data URI Scheme](https://en.wikipedia.org/wiki/Data_URI_scheme), [CSS Tricks, Data URIs](https://css-tricks.com/data-uris/)

### Document Object Model (DOM)

The DOM is an API that is constructed from markup (HTML, XHTML or XML) parsed as nodes in a tree format to allow programmatic access through scripting.

Reference: [DOM Enlightenment, Chapter 1](https://domenlightenment.com)

### `Node`

> ... an interface from which a number of DOM API object types inherit

A list of them can be found in the `Node` object:

```JavaScript
Object.keys(Node);

// Firefox Developer Edition 61
// [
//   "ELEMENT_NODE",
//   "ATTRIBUTE_NODE",
//   "TEXT_NODE",
//   "CDATA_SECTION_NODE",
//   "ENTITY_REFERENCE_NODE",
//   "ENTITY_NODE",
//   "PROCESSING_INSTRUCTION_NODE",
//   "COMMENT_NODE",
//   "DOCUMENT_NODE",
//   "DOCUMENT_TYPE_NODE",
//   "DOCUMENT_FRAGMENT_NODE",
//   "NOTATION_NODE",
//   "DOCUMENT_POSITION_DISCONNECTED",
//   "DOCUMENT_POSITION_PRECEDING",
//   "DOCUMENT_POSITION_FOLLOWING",
//   "DOCUMENT_POSITION_CONTAINS",
//   "DOCUMENT_POSITION_CONTAINED_BY",
//   "DOCUMENT_POSITION_IMPLEMENTATION_SPECIFIC"
// ]
```

Each of the interfaces/constructors are inherited from a chain of ancestors; for example, the ancestor chain of the anchor element is `Object < EventTarget < Node < Element < HTMLElement < HTMLAnchorElement`.

References: [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/Node), [DOM Enlightenment, Chapter 1](https://domenlightenment.com)

## Patterns

### Ancestor Chain of a DOM Node

One way to enquire about this through JavaScript, the following snippet is tested on Firefox Developer Edition 61 and should work on most modern browsers:

```JavaScript
function getAncestors(obj) {
  let ancestor = Object.getPrototypeOf(obj);

  if (ancestor) {
    console.log(ancestor.constructor.name);
    getAncestors(ancestor);
  }
}

let anchor = document.createElement('a');

getAncestors(anchor);

// HTMLAnchorElement
// HTMLElement
// Element
// Node
// EventTarget
// Object
```

## Questions

### Discuss various methods for programatically creating, cloning, inserting, replacing, and removing DOM nodes, text and elements.

`Placeholder`

### How does one iterate through a `NodeList` or an `HTMLCollection`?

`NodeList` and `HTMLCollection` are array-like objects, it must first be converted to an array with `Array.from` or `[].slice.call()`.

Reference: [DOM Enlightenment, Chapter 1](https://domenlightenment.com)

### How does one get a string representation of the HTML content of an element?

`Element.innerHTML`. It is worth noting that `Node.textContent` can be used to get the, well, text content inside a node; in the case that the inner HTML of an element also contains other nodes, `Node.textContent` will omit the child nodes and return everything else textual.

### How does one get a list of all classes on an element? Can it be modified?

`Element.classList`. The array can be modified using the `add()` and `remove()`, `contains()` and `toggle()` methods.

Reference: [DOM Enlightenment, Chapter 3](https://domenlightenment.com)

One can also get the classes of an element as a string with the `Element.className` attribute, assigning a new string to this attribute can overwrite its existing classes.

### How does one access and modify the `data-*` attributes of an element?

`HTMLELement.dataset` or `Element.getAttribute` and `Element.setAttribute`.

### List at least four ways for selecting DOM element(s) using JavaScript. (Potential follow-up question: what are the differences between the methods available to the `document` object and those on an element?)

`document.getElementsByTagName()`, `document.getElementsByClassName()`, `document.getElementById()`, `document.querySelector()`/`document.querySelectorAll()`, `Element.getElementsByTagName`, `Element.getElementsByTagName`, `Element.querySelector()`/`Element.querySelectorAll()`.

### What are some methods for improving the performance of a website?

* Write clean, organised code in general
* Write well architected CSS (short, low-specificity selectors, a focus on modular/reusable classes)
* gzip compression
* Image compression
* Combine files (such as CSS and JavaScript files, using a sprite sheet for images)
* Image data URI (relate to SVG)

Reference: [Learn to Code Advanced HTML & CSS](https://learn.shayhowe.com/advanced-html-css/performance-organization/)

### What are some methods for traversing DOM nodes?

`Node.parentNode`, `Node.childNodes`, `Node.firstChild`, `Node.lastChild`, `Node.previousSibling`, `Node.nextSibling`, `ParentNode.firstElementChild`, `ParentNode.lastElementChild`, `ParentNode.previousElementChild`, `ParentNode.nextElementChild`, `ParentNode.children`.

Reference: [DOM Enlightenment, Chapter 1](https://domenlightenment.com)

### What is a potential problem with `Node.cloneNode()`?

`Placeholder`

## Resources

* [DOM Enlightenment](https://domenlightenment.com/)
