# CSS

## Table of Contents

* [CSS](#html)
  * [Terminology](#terminology)
  * [Patterns](#patterns)
  * [Questions](#questions)

## Terminology

### Box Model

The model based on which the layout of an HTML elements is calculated, where:

> every element on a page is a rectangular box and may have width, height, padding, borders, and margins.

In most modern browsers, opening the inspector and selecting an element would gives detailed information about the element, which usually includes a section that is a visual representation of the box model for that element.

Reference: [Learn to Code HTML & CSS, Lesson 4](https://learn.shayhowe.com/html-css/opening-the-box-model/#how-are-elements-displayed)

### Clearing Floats

The use of the CSS rule `clear: [left, right, both]` to remove the irregular flow after `float`ed elements and return them to the normal/intended flow.

Reference: [Learn to Code HTML & CSS, Lesson 5](https://learn.shayhowe.com/html-css/positioning-content/)

### CSS Reset

A CSS reset removes or normalises the CSS predefined CSS rules implemented by different browsers so that a designer/web developer can start from a common baseline for a unified experience across the targeted browsers (provided that any additional CSS rules added after resetting are compatible with the targeted browsers).

Reference: [Learn to Code HTML & CSS, Lesson 1](https://learn.shayhowe.com/html-css/building-your-first-web-page/)

### Font Stack

A set values representing fonts, ordered in decreasing **intended** importance from left to right, assigned to the `font-family` CSS property. A font to the right of another is intended to be a fallback for the font to its left, should font to its left be not available for any reason.

### Key Selector

The selector immediately before an opening curly bracket, which "identifies exactly which element the styles will be applied to."

Reference: [Learn to Code HTML & CSS, Lesson 3](https://learn.shayhowe.com/html-css/getting-to-know-css/)

### Prequalifier

Selector(s) that appear before (to the left) of the [key selector](#key-selector).

Reference: [Learn to Code HTML & CSS, Lesson 3](https://learn.shayhowe.com/html-css/getting-to-know-css/)

### Specificity

The relative importance of a CSS declaration that is determined by its selector or combination of selectors. The specificity of a selector is represented as a three-value point system; with the lowest level specificity represented as `0-0-1` (for example, a type selector), the medium level of specificity represented as `0-1-0` (for example, a class selector), and the highest level of specificity represented as `1-0-0` (for example, an id selector). Specificity is additive.

Reference: [Learn to Code HTML & CSS, Lesson 3](https://learn.shayhowe.com/html-css/getting-to-know-css/)

### Selector

> ... designates exactly which element or elements within our HTML to target and apply styles (such as color, size, and position) to.

Reference: [Learn to Code HTML & CSS, Lesson 1](https://learn.shayhowe.com/html-css/building-your-first-web-page/)

## Patterns

### Centering a Background Image

Note: that this pattern is anti-semantic.

```CSS
.img {
  height: 200px;
  width: 200px;
  background-position: 50% 50%;
  background-repeat: no-repeat;
  background-size: cover;
}
```

### Clearfix with Support for old IEs

```CSS
.container::before,
.container::after {
  content: "";
  display: table;
}
.container::after {
  clear: both;
}
.container {
  clear: both;
  *zoom: 1;
}
```

### Clearfix without Support for old IEs

```CSS
.container::after {
  content: "";
  display: block;
  clear: both;
}
```

Reference: [Stack Overflow](https://stackoverflow.com/questions/211383/what-methods-of-clearfix-can-i-use/1633170#1633170), the accepted solution is great.

## Question

### Briefly describe what HTML elements are used for handling audio and video, their commonly used attributes. What potential problems you may run into with audio and video on a web page? How would you handle them?

`<audio>`, `<video>` elements! Surprise! Some common attributes include `src`, `type`, `control` (boolean), `autoplay` (boolean).

### Describe the uses of the various types of assignable to the `position` CSS property.

`Placeholder`

### Discuss the advantages and disadvantages of using web fonts. For the disadvantages mentioned, what are some potential solutions?

`Placeholder`

### In terms of the box model, what is the difference between an `inline` element and an `inline-block` element?

`Placeholder`

### Helvetica is a popular font. Is it web safe?

`Placeholder`

### How does one create a download link?

`Placeholder`

### How does one get rid of unwanted space between `inline` elements?

`Placeholder`

### Should one drop units for 0 length values?

I personally don't think so because it's actually easier to read and distinguish multiple values. At the end of the day, it's a stylistic issue and our brains register things differently—and, at least for me `0px 42px` is much clearer to me than `0 42px`. In my opinion, there is no good or bad in this particular case; be consistent with the codebase that you are working on and choose whatever you want for your own projects (also be consistent).

### What does the `box-sizing: border-box` CSS rule do? What are the advantages and disadvantages of this CSS rule? (Potential trick, follow-up question: what about `padding-box`?)

`Placeholder`

### What is the difference between the `em` and `rem` CSS length units?

`Placeholder`

### What are some problems with creating layouts using `float`s? What are the solutions? What other CSS rules would you use in place of float? Give illustrated examples.

`Placeholder`

### What are the `rgba` values of the keyword colour `red`? What is corresponding hexadecimal representation? How would you adjust the values to give a pinkish colour (just a lighter shade of red—not the keyword colour `pink`)?

`Placeholder`

### What are CSS resets? What are the advantages and disadvantages of using CSS resets?

`Placeholder`

### What value does a font stack usually end with?

`Placeholder`

### Why is it often a good idea to keep specificity of a selector, or a combination of selectors, low?

`Placeholder`

## Resources

* [W3C CSS Validation Service](https://jigsaw.w3.org/css-validator/)
* [Can I use](https://caniuse.com/)
* [Learn to Code HTML & CSS, Lesson 12, Additional Resources and Links](https://learn.shayhowe.com/html-css/writing-your-best-code/)
