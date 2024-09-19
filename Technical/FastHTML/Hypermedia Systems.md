
1. What is a hypermedia control ?

	Consider a simple anchor tag, embedded within a larger HTML document:
	
	```html
	<a href="https://hypermedia.systems/">
	  Hypermedia Systems
	</a>
	```
	
	An anchor tag consists of the tag itself, `<a></a>`, as well as the attributes and content within the tag. Of particular interest is the `href` attribute, which specifies a _hypertext reference_ to another document or document fragment. It is this attribute that makes the anchor tag a hypermedia control.

2. ```
────────────────────────┐      ┌─HTTP REQUEST────────────────┐
│ BROWSER              X │      │                             │
├────────────────────────┤      │ GET /                       │
│                        │      │ Host: hypermedia.systems    │
│ lorem ipsum dolor      │      └─────────────────────────────┘
│                        │
│ Hypermedia Systems ────┼───────────┐
│ ──────────────────     │           │
│ sit amet               │           │
│                        │           │
└────────────────────────┘           │
                              ┌──────▼──────┐
                              │   H T T P   │
                              │ S E R V E R │
                              └──────┬──────┘
┌────────────────────────┐           │
│ BROWSER              X │           │
├────────────────────────┤           │
│                        │           │
│ HYPERMEDIA SYSTEMS     ◀───────────┘
│                        │
│ The revolutionary      │      ┌─HTTP RESPONSE───────────────┐
│                        │      │                             │
│ ideas that empowered...│      │ 200 OK                      │
│                        │      │ ...                         │
└────────────────────────┘      │ <h1>Hypermedia Systems</h1> │
                                │ ...                         │```

3. In a typical web browser, this anchor tag would be interpreted to mean
   
   1. Show the text “Hypermedia Systems” in a manner indicating that it is clickable
    
   2. When the user clicks on that text, issue an HTTP `GET` request to the URL `https://hypermedia.systems/`
    
   3.  Take the HTML content in the body of the HTTP response to this request and replace the entire screen in the browser as a new document, updating the navigation bar to this new URL.
4. Form tags

Anchor tags provide _navigation_ between documents or resources, but don’t allow you to update those resources. That functionality falls to the form tag.

Here is a simple example of a form in HTML:

```html
<form action="/signup" method="post">
  <input type="text" name="email" placeholder="Enter Email To Sign Up">
  <button>Sign Up</button>
</form>
```

A simple form

Like an anchor tag, a form tag consists of the tag itself, `<form></form>`, combined with the attributes and content within the tag. Note that the form tag does not have an `href` attribute, but rather has an `action` attribute that specifies where to issue an HTTP request.

Furthermore, it also has a `method` attribute, which specifies exactly which HTTP “method” to use. In this example the form is asking the browser to issue a `POST` request.

In contrast with anchor tags, the content and tags _within_ a form can have an effect on the hypermedia interaction that the form makes with a server. The _values_ of `input` tags and other tags such as `select` tags will be included with the HTTP request when the form is submitted, as URL parameters in the case of a `GET` and as part of the request body in the case of a `POST`. This allows a form to include an arbitrary amount of information collected from a user in a request, unlike the anchor tag.

In a typical browser this form tag and its contents would be interpreted by the browser roughly as follows:

- Show a text input and a “Sign Up” button to the user
    
- When the user submits the form by clicking the “Sign Up” button or by hitting the enter key while the input element is focused, issue an HTTP `POST` request to the path `/signup` on the “current” server
    
- Take the HTML content in the body of the HTTP response body and replace the entire screen in the browser as a new document, updating the navigation bar to this new URL.
    

This mechanism allows the user to issue requests to _update the state_ of resources on the server. Note that despite this new type of request the communication between client and server is still done entirely with _hypermedia_.

It is the form tag that makes Hypermedia-Driven Applications possible.

```
┌────────────────────────┐      ┌─HTTP REQUEST────────────────┐
│ BROWSER              X │      │                             │
├────────────────────────┤      │ POST /                      │
│                        │      │ Host: hypermedia.systems    │
│ SIGN UP                │      │ ...                         │
│ ┌────────────────────┐ │      │ email=joe@example.com       │
│ │ joe@example.com    │ │      └─────────────────────────────┘
│ └────────────────────┘ │
│ ┌─────────┐        ────┼───────────┐
│ │ Sign up │            │           │
│ └─────────┘            │           │
└────────────────────────┘           │
                              ┌──────▼──────┐
                              │   H T T P   │
                              │ S E R V E R │
                              └──────┬──────┘
┌────────────────────────┐           │
│ BROWSER              X │           │
├────────────────────────┤           │
│                        │           │
│ THANK YOU FOR SIGNING  ◀───────────┘
│ UP                     │
│                        │      ┌─HTTP RESPONSE───────────────┐
│                        │      │                             │
│                        │      │ 201 Created                 │
│                        │      │ ...                         │
└────────────────────────┘      │ <h1>Thank you for signing   │
                                │ up</h1>                     │
                                └─────────────────────────────┘
```

5. consider the fact that these two hypermedia controls, anchors and forms, are the _only_ native ways for a user to interact with a server in plain HTML.
6. ![htmx advantage](Hypermedia%20Systems-image-1.png)
7. By adopting the hypermedia approach for these applications, you will save yourself a huge amount of client-side complexity that comes with adopting the Single Page Application approach: there is no need for client-side routing, for managing a client-side model, for hand-wiring in JavaScript logic, and so forth. The back button will “just work.” Deep linking will “just work.” You will be able to focus your efforts on your server, where your application is actually adding value.
8. A **uniform resource locator** is a textual string that refers to, or _points to_ a location on a network where a _resource_ can be retrieved from, as well as the mechanism by which the resource can be retrieved. A URL is a string consisting of various subcomponents:    [scheme]://[userinfo]@[host]:[port][path]?[query]#[fragment]
9. Note that URLs are often not written out entirely within HTML. It is very common to see anchor tags that look like this, for example:

```html
<a href="/book/contents/">Table Of Contents</a>
```



10. Here we have a _relative_ hypermedia reference, where the protocol, host and port are _implied_ to be that of the “current document,” that is, the same as whatever the protocol and server were to retrieve the current HTML page. So, if this link was found in an HTML document retrieved from `https://hypermedia.systems/`, then the implied URL for this anchor would be`https://hypermedia.systems/book/contents/`.
11. The cache behavior of an HTTP response from a server can be indicated with the `Cache-Control` response header. This header can have a number of different values indicating the cacheability of a given response. If, for example, the header contains the value `max-age=60`, this indicates that a client may cache this response for 60 seconds, and need not issue another HTTP request for that resource until that time limit has expired.
12. building a Hypermedia-Driven Application gives you a lot more freedom in picking the back end technology you want to use. Your decision can be based on the domain of your application, what languages and server software you are familiar with or are passionate about, or just what you feel like trying out.
13. Browsers aren’t the only hypermedia clients out there, however. Hyperview, a mobile-oriented hypermedia. One of the outstanding features of Hyperview is that it doesn’t simply provide a hypermedia, HXML, but also provides a _working hypermedia client_ for that hypermedia. This makes building a proper Hypermedia-Driven Application with Hyperview extremely easy.
14. Here are the constraints of REST Fielding outlines:
	- It is a client-server architecture
	- It must be stateless; that is, every request contains all information necessary to respond to that request.
	- It must allow for caching.
	- It must have a _uniform interface
	- It is a layered system
	- Optionally, it can allow for Code-On-Demand, that is, scripting.
15. In practice, for many web applications today, we actually violate "stateless" constraint: it is common to establish a _session cookie_ that acts as a unique identifier for a given user and that is sent along with every request. While this session cookie is, by itself, not stateful (it is sent with every request), it is typically used as a key to look up information stored on the server, in what is usually termed “the session.” This session information is typically stored in some sort of shared storage across multiple web servers, holding things like the current user’s email or id, their roles, partially created domain objects, caches, and so forth.
16. These operations correspond closely to the CRUD operations. By giving us access to only two of the five, HTML hamstrings our ability to take full advantage of HTTP.
	- `GET` corresponds with “getting” a representation for a resource from a URL: it is a pure read, with no mutation of the resource.
	    
	- `POST` submits an entity (or data) to the given resource, often creating or mutating the resource and causing a state change.
	    
	- `PUT` submits an entity (or data) to the given resource for update or replacement, again likely causing a state change.
	    
	- `PATCH` is similar to `PUT` but implies a partial update and state change rather than a complete replacement of the entity.
	    
	- `DELETE` deletes the given resource.

17. Htmx is a JavaScript library that extends HTML in exactly this manner. Again, htmx is not the only JavaScript library that takes this hypermedia-oriented approach (other excellent examples are [Unpoly](https://unpoly.com/) and [Hotwire](https://hotwire.dev/)), but htmx is the purest in its pursuit of extending HTML as a hypermedia.
18. It turns out that the default htmx behavior is to simply put the returned content inside the element that triggered the request. That’s _not_ a good thing in the case of our button: we will end up with a list of contacts awkwardly embedded within the button element. That will look pretty silly and is obviously not what we want.

	Fortunately htmx provides another attribute, `hx-target` which can be used to specify exactly _where_ in the DOM the new content should be placed. The value of the `hx-target` attribute is a Cascading Style Sheet (CSS) _selector_ that allows you to specify the element to put the new hypermedia content into.
	
	Let’s add a `div` tag that encloses the button with the id `main`. We will then target this `div` with the response:

```html
<div id="main"> <1>

  <button hx-get="/contacts" hx-target="#main"> <2>
    Get The Contacts
  </button>

</div>
```

19. By using CSS selectors, htmx builds on top of familiar and standard HTML concepts. This keeps the additional conceptual load for working with htmx to a minimum.

	Given this new configuration, what would the HTML on the client look like after a user clicks on this button and a response has been received and processed?
	
	It would look something like this:

```html
<div id="main">
  <ul>
    <li><a href="mailto:joe@example.com">Joe</a></li>
    <li><a href="mailto:sarah@example.com">Sarah</a></li>
    <li><a href="mailto:fred@example.com">Fred</a></li>
  </ul>
</div>
```

	Our HTML after the htmx request finishes

20. Now, perhaps we don’t want to load the content from the server response _into_ the div, as child elements. Perhaps, for whatever reason, we wish to _replace_ the entire div with the response. To handle this, htmx provides another attribute, `hx-swap`, that allows you to specify exactly _how_ the content should be swapped into the DOM.

	The `hx-swap` attribute supports the following values:
	
	- `innerHTML` - The default, replace the inner html of the target element.
	    
	- `outerHTML` - Replace the entire target element with the response.
	    
	- `beforebegin` - Insert the response before the target element.
	    
	- `afterbegin` - Insert the response before the first child of the target element.
	    
	- `beforeend` - Insert the response after the last child of the target element.
	    
	- `afterend` - Insert the response after the target element.
	    
	- `delete` - Deletes the target element regardless of the response.
	    
	- `none` - No swap will be performed.
	
21. To do so would require only a small change to our button, adding a new `hx-swap` attribute:

```html
<div id="main">

  <button hx-get="/contacts" hx-target="#main" hx-swap="outerHTML"> <1>
    Get The Contacts
  </button>

</div>
```

	Replacing the entire div
	
	1. The `hx-swap` attribute specifies how to swap in new content.
    

Now, when a response is received, the _entire_ div will be replaced with the hypermedia content:

```html
<ul>
  <li><a href="mailto:joe@example.com">Joe</a></li>
  <li><a href="mailto:sarah@example.com">Sarah</a></li>
  <li><a href="mailto:fred@example.com">Fred</a></li>
</ul>
```

	Our HTML after the htmx request finishes

22. htmx generalizes this notion of an event triggering a request by using, you guessed it, another attribute: `hx-trigger`. The `hx-trigger` attribute allows you to specify one or more events that will cause the element to trigger an HTTP request.

	Often you don’t need to use `hx-trigger` because the default triggering event will be what you want. The default triggering event depends on the element type, and should be fairly intuitive:
	
	- Requests on `input`, `textarea` & `select` elements are triggered by the `change` event.
	    
	- Requests on `form` elements are triggered on the `submit` event.
	    
	- Requests on all other elements are triggered by the `click` event.
    

	To demonstrate how `hx-trigger` works, consider the following situation: we want to trigger the request on our button when the mouse enters it. Now, this is certainly not a _good_ UX pattern, but bear with us: we are just using this as an example.
	
	To respond to a mouse entering the button, we would add the following attribute to our button:

```html
<div id="main">

  <button hx-get="/contacts" hx-target="#main" hx-swap="outerHTML"
    hx-trigger="mouseenter"> <1>
    Get The Contacts
  </button>

</div>
```

23. Let’s try something a bit more realistic and potentially useful: let’s add support for a keyboard shortcut for loading the contacts, `Ctrl-L` (for “Load”). To do this we will need to take advantage of additional syntax that the `hx-trigger` attribute supports: event filters and additional arguments.

	Event filters are a mechanism for determining if a given event should trigger a request or not. They are applied to an event by adding square brackets after it: `someEvent[someFilter]`. The filter itself is a JavaScript expression that will be evaluated when the given event occurs. If the result is truthy, in the JavaScript sense, it will trigger the request. If not, the request will not be triggered.
	
	In the case of keyboard shortcuts, we want to catch the `keyup` event in addition to the click event:
	
	```html
	<div id="main">
	
	  <button hx-get="/contacts" hx-target="#main" hx-swap="outerHTML"
	    hx-trigger="click, keyup"> <1>
	    Get The Contacts
	  </button>
	
	</div>
	```    
	
	Note that we have a comma separated list of events that can trigger this element, allowing us to respond to more than one potential triggering event. We still want to respond to the `click` event and load the contacts, in addition to handling the `Ctrl-L` keyboard shortcut.
	
	Unfortunately there are two problems with our `keyup` addition: As it stands, it will trigger requests on _any_ keyup event that occurs. And, worse, it will only trigger when a keyup occurs _within_ this button. The user would need to tab onto the button to make it active and then begin typing.
	
	Let’s fix these two issues. To fix the first one, we will use a trigger filter to test that Control key and the “L” key are pressed together:
	
	```html
	<div id="main">
	
	  <button hx-get="/contacts" hx-target="#main" hx-swap="outerHTML"
	    hx-trigger="click, keyup[ctrlKey && key == 'l']"> <1>
	    Get The Contacts
	  </button>
	
	</div>
	```
	
	The trigger filter in this case is `ctrlKey && key == 'l'`. This can be read as “A key up event, where the ctrlKey property is true and the key property is equal to l.” Note that the properties `ctrlKey` and `key` are resolved against the event rather than the global name space, so you can easily filter on the properties of a given event. You can use any expression you like for a filter, however: calling a global JavaScript function, for example, is perfectly acceptable.
	
	OK, so this filter limits the keyup events that will trigger the request to only `Ctrl-L` presses. However, we still have the problem that, as it stands, only `keyup` events _within_ the button will trigger the request.
	
	If you are not familiar with the JavaScript event bubbling model: events typically “bubble” up to parent elements. So an event like `keyup` will be triggered first on the focused element, and then on its parent (enclosing) element, and so on, until it reaches the top level `document` object that is the root of all other elements.
	
	To support a global keyboard shortcut that works regardless of what element has focus, we will take advantage of event bubbling and a feature that the `hx-trigger` attribute supports: the ability to listen to _other elements_ for events. The syntax for doing this is the `from:` modifier, which is added after an event name and that allows you to specify a specific element to listen for the given event on using a CSS selector.
	
	In this case, we want to listen to the `body` element, which is the parent element of all visible elements on the page.
	
	Here is what our updated `hx-trigger` attribute looks like:
	
	```html
	<div id="main">
	
	  <button hx-get="/contacts" hx-target="#main" hx-swap="outerHTML"
	    hx-trigger="click, keyup[ctrlKey && key == 'l'] from:body"> <1>
	    Get The Contacts
	  </button>
	
	</div>
	```
	
	Even better, listen for keyup on the body
	
	1. Listen to the ‘keyup” event on the `body` tag.
	    
	
	Now, in addition to clicks, the button will listen for `keyup` events on the body of the page. So it will issue a request when it is clicked on and also whenever someone hits `Ctrl-L` within the body of the page.
	
	And now we have a nice keyboard shortcut for our Hypermedia-Driven Application.
	
	The `hx-trigger` attribute supports many more modifiers, and it is more elaborate than other htmx attributes. This is because events, in general, are complicated and require a lot of details to get just right. The default trigger will often suffice, however, and you typically don’t need to reach for complicated `hx-trigger` features when using htmx.

24. Summary of HTMX : HTML Extended
    1. Any element should be able to make HTTP requests`hx-get`, `hx-post`, `hx-put`, `hx-patch`, `hx-delete`
    2. Any event should be able to trigger an HTTP request`hx-trigger`
    3. Any HTTP Action should be available`hx-put`, `hx-patch`, `hx-delete`
    4. Any place on the page should be replaceable (transclusion)`hx-target`, `hx-swap`
25. The simplest way to pass input values with a request in htmx is to enclose the element making a request within a form tag.

	Let’s take our original search form and convert it to use htmx instead:
	
	```html
	<form action="/contacts" method="get" class="tool-bar"> <1>
	  <label for="search">Search Term</label>
	  <input id="search" type="search" name="q" 
	    value="{{ request.args.get('q') or '' }}"
	    placeholder="Search Contacts"/>
	  <button hx-post="/contacts" hx-target="#main"> <2>
	    Search
	  </button>
	</form>
	```
	
	An htmx-powered search button
	
	1. When an htmx-powered element is withing an ancestor form tag, all input values within that form will be submitted for non-`GET` requests
	    
	2. We have switched from an `input` of type `submit` to a `button` and added the `hx-post` attribute
	    
	
	Now, when a user clicks on this button, the value of the input with the id `search` will be included in the request. This is by virtue of the fact that there is a form tag enclosing both the button and the input: when an htmx-driven request is triggered, htmx will look up the DOM hierarchy for an enclosing form, and, if one is found, it will include all values from within that form. (This is sometimes referred to as “serializing” the form.)

