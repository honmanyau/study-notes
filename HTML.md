# HTML

## Table of Contents

* [HTML](#html)
  * [Terminology](#terminology)
  * [Questions](#questions)

## Terminology

### HTML Entities

> a piece of text ("string") that begins with an ampersand (&) and ends with a semicolon (;) . Entities are frequently used to display reserved characters (which would otherwise be interpreted as HTML code), and invisible characters (like non-breaking spaces).

Reference: [MDN Web Docs, Entity](https://developer.mozilla.org/en-US/docs/Glossary/Entity)

### Global DOM Variables

Variables declared in the global scope when working with the DOM; they are also added as a property to the `window` global object, which is accessible through JavaScript using the `window` object of the Web API. For example:

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

### Semantic HTML Element

An HTML element with a name that describes the element and its content's intended purpose within a document.

> There are several benefits to using semantic elements, including enabling computers, screen readers, search engines, and other devices to adequately read and understand the content on a web page.

HTML written with semantic elements are also easier to maintain simply because their names signal their purpose.

## Question

### Name at least 4 distinctly different types of length units, describe their uses and merits briefly.

`Placeholder`

### How and where in an HTML document do you link to an external style sheet? Why is it a good idea to to link to an external style sheet in that part of the document?

Partial answer: `<link rel="stylesheet" href="style.css">`.

### What are the roles of HTML and CSS in document? (Potential devious follow-up question: can you think of a case where their roles overlap?)

HTML is responsible for the structure and content of a document whereas CSS is responsible for styling the content.

### What are the benefits of using semantic HTML elements? (Potential follow-up question: can you name some common semantic HTML elements and uses?)

`Placeholder`

### What are the similarities and differences between the elements `<b>` and `<strong>`/`<i>` and `<em>`?

The `<b>` and `<i>` elements are stylistic descriptions of the content that they hold, they do not add any meaning to them. The semantic elements `<strong>` and `<em>` do style their content just like their non-semantic counterparts would; however, here the **meaning** that they give to their content is the focus (consider screen readers) and the styling (bold and italic, respectively) is just a secondary, visual result of that.

Reference: [Learn to Code HTML & CSS, Lesson 2](https://learn.shayhowe.com/html-css/getting-to-know-html/)

## Resources

* [W3C Markup Validation Service](https://validator.w3.org/)
* [W3C CSS Validation Service](https://jigsaw.w3.org/css-validator/)
* [HTML Entities](https://dev.w3.org/html5/html-author/charref)
