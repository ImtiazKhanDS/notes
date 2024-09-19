
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

17. 
    

