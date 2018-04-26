# Front End

## HTML
* [`<base>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base): specifies the base URL and base target for all relative URLs in a page.
* [`<pre>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/pre): text within this element is typically displayed in a non-proportional font exactly as it is laid out in the file.
* `usemap` attr in img tag: the partial URL (starting with '#') of an image map associated with the element.
* [`<map>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/map): used with `<area>` elements to define an image map (a clickable link area).

### HTML5 New Tags
* [html5shiv](https://github.com/aFarkas/html5shiv/): enable use of HTML5 sectioning elements in legacy Internet Explorer.

#### Semantic Layout
```
-----------------------
|      <header>       |
|---------------------|
|       <nav>         |
|---------------------|
| <section> | <aside> |
|-----------|         |
| <article> |         |
|---------------------|
|      <footer>       |
-----------------------
```

## Event Delegation
Event delegation is a technique involving adding event listeners to a **parent** element instead of adding them to the descendant elements. The listener will fire whenever the event is **triggered** on the **descendant** elements due to event **bubbling up** the DOM. The benefits of this technique are:

1. Memory footprint goes down because only **one single handler** is needed on the parent element, rather than having to attach event handlers on each descendant.

2. There is no need to unbind the handler from elements that are removed and to bind the event for new elements.

## [Application Cache](https://developer.mozilla.org/en-US/docs/Web/HTML/Using_the_application_cache)


## CSS3 New Features
### Prefix
For supporting the attribute before that become to a W3C recommended.

*  check at [shouldiprefix](http://shouldiprefix.com/).
*  develop with [autoprefixer](https://github.com/postcss/autoprefixer).


## Design
* 216 Web Safe Color: cross-browser color palette was created to ensure that all computers would display colors correctly.


