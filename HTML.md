# JavaScript

## Table of Contents

* [JavaScript](#javascript)
  * [Terminology](#terminology)

## Terminology

### Global DOM Variables

Variables declared in the global scope when working with the DOM; they are also added as a property to the `window` global object. For example:

```JavaScript
var nyanpasu = 'nyanpasu';

window.nyanpasu; // "nyanpasu"
```

In addition, the `id` attribute of a DOM element is also added to the global object, whose value is the DOM element itself. For example:

```HTML
<div id="nyanpasu"></div>
```

```JavaScript
window.nyanpasu; // HTML element, <div id="nyanpasu">
```

Reference: [YDKJS, Types & Grammar, Appendix A](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/apA.md)
