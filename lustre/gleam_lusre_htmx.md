#  HTMX, Gleam, Lustre

*Interested in greener pastures, I embark on a journey to create a viable alternative to current frameworks.*

*An Important note before reading: Thankyou to both teams behind gleam and lustre. And thankyou the person who created lustre for answering my unhinged questions.*

*Also, it is my understanding that what I have created here does not meet manufacture specifications for lustre. As such, expect that I may have introduced some flaws of my own into lustre, so please try it for your self based off off official tutorials.*

## HTMX
HTMX is a html extension written in javascript. HTMX exposes triggers, actions, and targets to place and replace the content of a page. Triggers fire actions which are requests to a server; this is just like html forms. The response from the server will contain some html this html is placed as specified by the target.

A user clicks a button (trigger) requesting the next page of products from `/products/p/4`. The server prepares and replies with html for page 4 of products. HTMX replaces the target, the product content, with a new page of products and the next button with one that has an action of `/products/p/5`.

<!-- include a diagram -->

<!-- include a code sample -->

## Gleam
Gleam is a functional programming language with two targets: a compilation to BEAM bytecode, a fault tolerant runtime originally used to route telecommunications; a transpilation to javascript, the language of the web.

One of the boons of a functional language is that unit testing is more effective, any result of a particular set of inputs for some function cannot be altered by another action happing before or during the execution of said function. That is functions that have the same input will always produce the same output.

There is one exception FFI, foreign function interface. This allows programers to expose functions written in javascript and erlang to gleam. The issue is functions of non-functional languages, called impure functions, have different properties. Impure functions are functions that when called with the same inputs may not result in the same output every time. Impure functions are often impure due to side effects that load and store memory. An example of an impure function is one that indicates how many times it has been called globally. A function with a result that depends on an impure function is also impure. This means that impure FFI can pollute the function space.

Since pure functions can not load or store memory the same way as pure functions functions will be written differently. Instead of passing a reference to an object that should be modified, pure functions will return a whole new modified version of an object. A function that deletes from a dictionary would take the dictionary and the key to delete; the function returns a new dictionary without that key. It is clear that this results in odd memory allocations and deletions. Functional languages may not be as fast as non-functional ones in some cases. However, as the predictability of pure functions will remove many development variables.

## Lustre
Lustre is a web framework which uses the model view controller architecture. When changes are made to the model the view function is re-called on the new model. The view defines which elements (in this case html) when interacted with will signal the controller. When signaled the controller will alter the model.

Lustre can either be compiled to beam and ran as a server or it can be compiled to javascript and embed itself into an html element when run. When compiled to javascript lustre can be used with HTMX to supplement HTMX's main weakness.

<!-- include the classic diagram -->

## The Framework
Unlike react and angular HTMX does not effectively store client side state. Instead HTMX relies on the server to provide the correct application state as requested. To avoid session based server code all information that the server needs should be provided in the request.

Most of this work is done by specifying actions at specific server routes. Such as getting the fourth page of products with `/products/p/4`. State can also be stored in the html itself when getting the fourth page the next button would be replaced by one that gets the fifth page.

The one thing that HTMX can not do is go offline. When cutoff from the server pages will not update. In most cases this is expected of web applications. However, there is a limited segment of work where web applications should be disconnection tolerant. Web editing sometimes called cloud. One of the most complicated example of this is web word processing in an application such as google docs or microsoft office 365. These types of application ironically do not require users to be online allowing for stints of disconnection.

Lustre allows for this kind functionality. Embedding lustre in HTMX resolves the client side weakness. Lustre applications once loaded do not require constant contact with a server to alter the webpage.

## How To Embed Lustre in HTMX

<!-- finish this -->
<!-- include important code samples: gleam, js ffi, htmx -->

## Unknowns
Is this better preforming then common frameworks?

Does the lustre application leak memory when reloaded?
