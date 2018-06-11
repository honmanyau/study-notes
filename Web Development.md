# Web Develoopment

## Table of Contents

* [Web Development](#web-development)
  * [Terminology](#terminology)
  * [Questions](#questions)

## Terminology

### Data URI

> ... a way to include data in-line in web pages as if they were external resources.

Caveats:

* The size of a data URI can be larger than that the resource(s) it is derived from.
* Potentially difficult to maintain
* Relies on caching

References: [Wikipedia, Data URI Scheme](https://en.wikipedia.org/wiki/Data_URI_scheme), [CSS Tricks, Data URIs](https://css-tricks.com/data-uris/)

## Questions

### What are some methods for improving the performance of a website?

* Write clean, organised code in general
* Write well architected CSS (short, low-specificity selectors, a focus on modular/reusable classes)
* gzip compression
* Image compression
* Combine files (such as CSS and JavaScript files, using a sprite sheet for images)
* Image data URI (relate to SVG)

Reference: [Learn to Code Advanced HTML & CSS](https://learn.shayhowe.com/advanced-html-css/performance-organization/)
