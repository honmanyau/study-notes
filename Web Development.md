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

Reference: [DOM Enlightenment, Chapter 1](https://domenlightenment.com), [Stack Overflow, What is event bubbling and capturing?](https://stackoverflow.com/questions/4616694/what-is-event-bubbling-and-capturing#4616720)

### Event Bubbling

Describes the propagation of events where they begin at the innermost element and traverses up/toward the trunk as the events propagate.

See also [Event Bubbling](#event-bubbling).

Reference: [DOM Enlightenment, Chapter 1](https://domenlightenment.com), [Stack Overflow, What is event bubbling and capturing?](https://stackoverflow.com/questions/4616694/what-is-event-bubbling-and-capturing#4616720)

### Event Capturing/Trickling

Describes the propagation of events where they begin at the outmost element and branch out to inner elements as the events propagate.

See also [Event Capturing](#event-capturing).

Reference: [DOM Enlightenment, Chapter 11](https://domenlightenment.com/)

### `Event.preventDefault()`

A method available to an object inherited from the `Event` interface that allows the default behaviour triggered by that event to be cancelled. For example, the default behaviour of `button` with the attribute and value `type="submit"` is to submit form data and refresh the page. By calling `Event.preventDefault` in an event handler triggered by an event listener for this type of event, the aforementioned behaviour can be prevented.

It is worth noting that `return`ing `false` inside an event handler produces the same result. In my opinion, however, the intent is less clear and probably should be avoided.

The `cancelable` property of an `Event` object indicates whether or not an event can be cancelled. The `defaultPrevented` property of an `Event` object indicates whether or not `Event.preventDefault()` has already been called anytime during propagation.

Reference: [DOM Enlightenment, Chapter 11](https://domenlightenment.com/)

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

### Event Capturing/Trickling and Bubbling

```HTML
<main id="game" class="game center-content">
  <article id="stage" class="stage center-content">
    <section id="field" class="field center-content">
      <div id="player" class="player">
        <aside class="description">#player</aside>
      </div>

      <aside class="description">#field</aside>
    </section>

    <aside class="description">#stage</aside>
  </article>

  <aside class="description">#game</aside>
</main>
```

```CSS
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
}

.center-content {
  display: flex;
  justify-content: center;
  align-items: center;
}

.game {
  height: 100vh;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #333;
  overflow: hidden;
}

.stage {
  height: 80%;
  width: 80%;
  position: relative;
  background: #CCC;
}

.field {
  height: 80%;
  width: 80%;
  position: relative;
  background: mediumSpringGreen;
}

.player {
  height: 100px;
  width: 100px;
  position: relative;
  background: crimson;
}

.description {
  padding: 5px 10px;
  position: absolute;
  top: 10px;
  left: 10px;
  border: 2px solid white;
  border-radius: 4px;
  font-family: sans-serif;
  font-size: 16px;
  color: white;
  background: rgba(0, 0, 0, 0.5);
}
```

```JavaScript
function handleBubbling(event) {
  console.log(`Bubbling: ${event.currentTarget.id}`);
}

function handleCapturing(event) {
  console.log(`Capturing/Trickling: ${event.currentTarget.id}`);
}


['game', 'stage', 'field', 'player'].forEach(function(id) {
  let element = document.getElementById(id);

  element.addEventListener('click', handleCapturing, true);
  element.addEventListener('click', handleBubbling, false);
});
```

### Event Simulation with the `CustomEvent()` Constructor

```HTML
<button id="nyanpasu">Nyanpasu!</button>
```

```JavaScript
const nyanpasu = document.getElementById('nyanpasu');
const customEvent = new CustomEvent('click', { detail: 'nyanpasu' });

function handleClick(event) {
  console.log(event.target.textContent);
}

nyanpasu.addEventListener('click', handleClick);
nyanpasu.dispatchEvent(customEvent); // "Nyanpasu!"
```

## Questions

### Describe how one would modify the background colour of an element programatically. (Potential follow-up question: what about `transform`?)

`Placeholder`

### Briefly discuss the difference between the `GlobalEventHandlers.onclick` and `EventTarget.addEventListener()`.

`Placeholder`

### Discuss various methods for programatically creating, cloning, inserting, replacing, and removing DOM nodes, text and elements.

`Placeholder`

### How does one access and modify the `data-*` attributes of an element?

`HTMLELement.dataset` or `Element.getAttribute` and `Element.setAttribute`.

### How does one append text to an element programatically without using `document.createElement`?

`Placeholder`

### How does one deal with adjacent text nodes?

`Node.normalize()`, `Node.textContent`.

### How does one get a string representation of the HTML content of an element?

`Element.innerHTML`. It is worth noting that `Node.textContent` can be used to get the, well, text content inside a node; in the case that the inner HTML of an element also contains other nodes, `Node.textContent` will omit the child nodes and return everything else textual.

### How does one get a list of all classes on an element? Can it be modified?

`Element.classList`. The array can be modified using the `add()` and `remove()`, `contains()` and `toggle()` methods.

Reference: [DOM Enlightenment, Chapter 3](https://domenlightenment.com)

One can also get the classes of an element as a string with the `Element.className` attribute, assigning a new string to this attribute can overwrite its existing classes.

### How does one iterate through a `NodeList` or an `HTMLCollection`?

`NodeList` and `HTMLCollection` are array-like objects, it must first be converted to an array with `Array.from` or `[].slice.call()`.

Reference: [DOM Enlightenment, Chapter 1](https://domenlightenment.com)

### How does one prevent the propagation of an event 1. vertically (through capturing or bubbling) and 2. horizontally (multiple event listeners on the same node)? (Potential follow-up question: what about only horizontally?)

`Event.stopPropagation()` and `Event.stopImmediatePropagation()`.

Reference: [DOM Enlightenment, Chapter 11](https://domenlightenment.com)

### How does one remove an event listener? (Potential follow-up question: what if the event handler is an anonymous function?)

`Placeholder`

### List at least four ways for selecting DOM element(s) using JavaScript. (Potential follow-up question: what are the differences between the methods available to the `document` object and those on an element?)

`document.getElementsByTagName()`, `document.getElementsByClassName()`, `document.getElementById()`, `document.querySelector()`/`document.querySelectorAll()`, `Element.getElementsByTagName`, `Element.getElementsByTagName`, `Element.querySelector()`/`Element.querySelectorAll()`.

### Using blocks of different colours, where each of them refer to "HTML Parsing", "HTML Parsing Paused", "Script Download" and "Script Execution", draw three diagrams, each of which demonstrates the difference between `<script>`, `<script defer>` and `<script async>` (hint: use two time domains for each diagram).

`Placeholder`

Reference: [Growing with the Web, async vs defer attributes](https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html)

### Using `Element.getBoundingClientRect()`, `Element.clientHeight` and `Element.clientWidth`, how does one get the size of of a `div` element both including and excluding the border?

`Placeholder`

### What is the easiest way to implement smooth scrolling to an element programatically? (Potential follow-up question: do you know of an easier way by using the Web API?)

`Placeholder`

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

### What are the differences between `Node.textContent` and `Node.innerText`?

`Placeholder`

Reference: [MDN Web Docs, `Node.textContent`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent)

### What is the difference between `Event.target` and `Event.currentTarget`? Illustrate with an example involving event propagation through the DOM tree.

`Placeholder`

### What is a potential problem with `Node.cloneNode()`?

`Placeholder`

### What is event delegation? Give an example for a table with thousands of entries, the data for which is dynamically generated and obtained requested from an external source.

`Placeholder`

### What is the value of `this` inside an event handler triggered by a DOM event?

The node that contains the event listener that triggered the event handler.

### Where should the `<script>` tag be placed in an HTML document and why? (Potential follow-up question: can you expand your answer by considering also the `defer` and `async` attributes?)

`Placeholder`

## Resources

* [DOM Enlightenment](https://domenlightenment.com/)
