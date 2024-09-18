
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
4.  