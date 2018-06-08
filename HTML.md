# HTML

## Table of Contents

* [HTML](#html)
  * [Terminology](#terminology)
  * [Patterns](#patterns)
  * [Questions](#questions)

## Terminology

### Describe two method for starting an ordered list at 42?

By using the `start` attribute of `<ol>`; or the `value` attribute of `<li>`.

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

### HTML Entities

> a piece of text ("string") that begins with an ampersand (&) and ends with a semicolon (;) . Entities are frequently used to display reserved characters (which would otherwise be interpreted as HTML code), and invisible characters (like non-breaking spaces).

Reference: [MDN Web Docs, Entity](https://developer.mozilla.org/en-US/docs/Glossary/Entity)

### Semantic HTML Element

An HTML element with a name that describes the element and its content's intended purpose within a document.

> There are several benefits to using semantic elements, including enabling computers, screen readers, search engines, and other devices to adequately read and understand the content on a web page.

HTML written with semantic elements are also easier to maintain simply because their names signal their purpose.

## Patterns

### Audio/Video Fallback

```HTML
<video controls>
  <source src="nyanpasu.ogv" type="video/ogg">
  <source src="nyanpasu.mp4" type="video/mp4">
  Nyanpasu! Please <a href="nyanpasu.mp4" download>download</a> the video!
</video>
```

### Semantic Figure and Caption

```HTML
<figure>
  <img src="nyanpasu.jpg">
  <figcaption>Figure 1. Nyanpasu!</figcaption>
</figure>
```

### Form Example

```HTML
<form>
  <fieldset>
    <legend>Login Credentials</legend>
    <label>
      Username
      <input type="text" name="username" required />
    </label>
    <label>
      E-mail address
      <input type="email" name="email" required />
    </label>
    <label>
      Password
      <input type="password" name="password" required />
    </label>
  </fieldset>
  <fieldset>
    <input type="submit" name="submit" value="Sign Up" />
    <input type="hidden" name="metadata" value="signup-1007" disabled />
    <p>By clicking on the Sign Up button, you agree to our terms and conditions.</p>
  </fieldset>
</form>
```

### Table Example

```HTML
<table>
  <caption>Table 16. Activation parameters calculated for the reaction between 6-methyl-2,3,4,5-tetrahydropyridine (<b>61</b>) and <i>n</i>-bromobutane (<b>59</b>). Errors are reported as standard errors from regression.</caption>
  <thead>
    <tr>
      <th></th>
      <th scope="col">ΔH<sup>‡</sup> / kJ·mol<sup>-1</sup></th>
      <th scope="col">ΔS<sup>‡</sup> / J·K<sup>-1</sup>·mol<sup>-1</sup></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Acetonitrile</td>
      <td>58.5 (8.9)</td>
      <td>-206 (27)</td>
    </tr>
    <tr>
      <td>[bmim][N(CF<sub>3</sub>SO<sub>2</sub>)<sub>2</sub>]</td>
      <td>85.0 (5.7)</td>
      <td>-112 (18)</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>Difference</td>
      <td>26.6(10.5)</td>
      <td>94 (33)</td>
    </tr>
  </tfoot>
</table>
```

Reference: [Learn to Code HTML & CSS, Lesson 9](https://learn.shayhowe.com/html-css/adding-media/)

## Question

### How do you disable an input element?

By using the `disabled` Boolean attribute.

### How and where in an HTML document do you link to an external style sheet? Why is it a good idea to to link to an external style sheet in that part of the document?

Partial answer: `<link rel="stylesheet" href="style.css">`.

### Name at least 4 distinctly different types of length units, describe their uses and merits briefly.

`Placeholder`

### What are the roles of HTML and CSS in document? (Potential devious follow-up question: can you think of a case where their roles overlap?)

HTML is responsible for the structure and content of a document whereas CSS is responsible for styling the content.

### What are the benefits of using semantic HTML elements? (Potential follow-up question: can you name some common semantic HTML elements and uses?)

`Placeholder`

### What are the similarities and differences between the elements `<b>` and `<strong>`/`<i>` and `<em>`?

The `<b>` and `<i>` elements are stylistic descriptions of the content that they hold, they do not add any meaning to them. The semantic elements `<strong>` and `<em>` do style their content just like their non-semantic counterparts would; however, here the **meaning** that they give to their content is the focus (consider screen readers) and the styling (bold and italic, respectively) is just a secondary, visual result of that.

Reference: [Learn to Code HTML & CSS, Lesson 2](https://learn.shayhowe.com/html-css/getting-to-know-html/)

### What does the `for` attribute do and on what element(s) is it commonly found? (Potential follow-up question: what can you say about the events triggered when an element with the `for` attribute is clicked on?)

`Placeholder`

## Resources

* [W3C Markup Validation Service](https://validator.w3.org/)
* [Can I use](https://caniuse.com/)
* [HTML Entities](https://dev.w3.org/html5/html-author/charref)
* [MDN Web Docs, HTML Attribute Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)
* [MDN Web Docs, <input>: The Input (Form Input) Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Input)
