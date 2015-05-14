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

## HTML5 Local Storage and Session Storage
* Store data locally within the user's browser.
* Can store large amount of data without affecting website performance.
* Nothing to do wih server.
* Each domain can only access to its own storage.

### Local Storage vs Cookies
cookies are added to every HTTP request header, which may end up having a measurable impact on response time.
In HTML5, we can use sessionStroage and localStroage in place of cookies.

sessionStroage: for the length of the session
localStroage: for persisting indefinitely

Both are properties of Window object.
Note: A different Storages object is used for the sessionStorageand localStorage for each origin.

Example using cookies as a fallback
Note: cookies are within document.cookie as plain string

``` javascript
if (!window.localStorage) {
  window.localStorage = {
    getItem: function (sKey) {
      if (!sKey || !this.hasOwnProperty(sKey)) { return null; }
      return unescape(document.cookie.replace(new RegExp("(?:^|.*;\\s*)" + escape(sKey).replace(/[\-\.\+\*]/g, "\\$&") + "\\s*\\=\\s*((?:[^;](?!;))*[^;]?).*"), "$1"));
    },
    key: function (nKeyId) {
      return unescape(document.cookie.replace(/\s*\=(?:.(?!;))*$/, "").split(/\s*\=(?:[^;](?!;))*[^;]?;\s*/)[nKeyId]);
    },
    setItem: function (sKey, sValue) {
      if(!sKey) { return; }
      document.cookie = escape(sKey) + "=" + escape(sValue) + "; expires=Tue, 19 Jan 2038 03:14:07 GMT; path=/";
      this.length = document.cookie.match(/\=/g).length;
    },
    length: 0,
    removeItem: function (sKey) {
      if (!sKey || !this.hasOwnProperty(sKey)) { return; }
      document.cookie = escape(sKey) + "=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/";
      this.length--;
    },
    hasOwnProperty: function (sKey) {
      return (new RegExp("(?:^|;\\s*)" + escape(sKey).replace(/[\-\.\+\*]/g, "\\$&") + "\\s*\\=")).test(document.cookie);
    }
  };
  window.localStorage.length = (document.cookie.match(/\=/g) || window.localStorage).length;
}

# Autosave contents of a text field
// Get the text field that we're going to track
var field = document.getElementById("field");
 
// See if we have an autosave value
// (this will only happen if the page is accidentally refreshed)
if (sessionStorage.getItem("autosave")) {
  // Restore the contents of the text field
  field.value = sessionStorage.getItem("autosave");
}
 
// Listen for changes in the text field
field.addEventListener("change", function() {
  // And save the results into the session storage object
  sessionStorage.setItem("autosave", field.value);
});
```

Reference: [DOM Storage guide](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage)


## Design
* 216 Web Safe Color: cross-browser color palette was created to ensure that all computers would display colors correctly.
