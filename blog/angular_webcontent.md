# this.blogTitle

Meta and tacky I know, but who does not like a bit of that. My personal experience with angular started in school. During the duration of the class I was not the most interested in Web, but ultimately I found that front end web was the only area where I could create a full project without too much hassle.

### Purpose of This Post
Describe the content and methods of [this webpage](https://benev0.github.io/). This page is not designed to be used as template or an example to create other pages, but there will be some demonstrations of solutions which other sources may push a "Angular" solution where it not capable. This post expects some understanding of what html, css, and js do.

*HTML (Hyper Text Markup Language)* - a system of formatting data to be rendered to the page by the browser

*CSS (Cascading Style Sheets)* - a system of manipulating how HTML is rendered

*JS (Java Script)* - Run code in the browser to manipulate HTML and CSS

## Angular
Angular is google's answer to dissatisfaction within web dev. Angular allows the client (the content on your web browser) to be handled right in front of you instead of on some server. Is that a good thing? No, it is strictly unneeded for most things. Things where it may be good to not have to contact the server every time you make a change are editors and nothing else. So google docs, office, and other cloud editing software are valid uses ironically because of the risk of disconnecting. On that note *Angular*!

### Components
An angular app consists of these. Components are a bundling of three things TS, aka Type Script which is like JS but cant run in tbe browser, HTML but actually not it has some template stuff in there too, and probably CSS Angular default HTML is still ugly. A component also has a selector which is the name that can be used like a standard html tag in other component's template HTML. Most of the work to create a component besides the copious boiler plate is the TS functionality this is defined in a TS class the (public) methods and fields that are created in this class may be used within the template HTML. There is one root component which contains the whole angular application.

{diagrams coming soon}

### Services
Services differ from Components in that there may be nay instances of a single component, per Service there is only one instance, services are singletons by default. Services use a Object Oriented pattern called dependency injection. All that means is that when you use a service in a component you do not need to manage the service, you do not need to create the service, you do not need to destroy the service, from the perspective of the component it exists, and you just call its methods.

{diagrams coming soon}

### Router
This is simply a special component which changes which component it points to based off of the page route (stuff after / in the url). This can all be done without reloading the page like a normal link.

{diagrams coming soon}

## this site's design

the core elements of this site are

0. the info page
1. the router and nav bar
2. the blog and blog navigation
3. the service to fetch the blogs
4. the pre-settings init process
5. the settings page & service

### info
As simple as they come this is a component that just hosts HTML content it is a component so that the router can target it. Info is the entry point of the site.

### nav bar
This is the primary control for this site the nav bar is simply a display of select routerlinks. Each one when clicked replacing the router component with the specified component.

### blog
The blog component has two parts the blog picker and the content if there is a subroute then the content for that blog or a 404 is displayed. The blog content and the list of available blogs must be requested from the blog service this service returns a promise which on completion updates the page with the requested content manifest or blog.

### blog service
This service returns promises for the blog manifest and blogs and caches the blog manifest. The blogs and blog manifest are fetched from a github repo via the raw file api. This is done via a Http client which runs in the browser.

### pre-init
This is a small script which indicates which version of CSS should be used to select either light or dark mode. The script checks the local storage for any content then, checks if the system (browser/os) has a preference otherwise the system will default to light mode. This is done to avoid any flicker on the user reloading the page. The pre-init script exists in the head of the base HTML and runs before any html is loaded thus preventing any style changes that would occur if this logic was ran in a component which is loaded with HTML.

```HTML
<!doctype html>
<html lang="en">
<head>
  <script>
    // retrieve session storage
    let storageResult = localStorage.getItem('DarkModePreferred');
    // parse session storage default to null
    var preferDark  = JSON.parse(storageResult ?? 'null');

    // if no session storage
    if (preferDark === null) {
      // try to get preference
      preferDark = window.matchMedia
        && window.matchMedia('(prefers-color-scheme: dark)').matches;
    }

    // if no preference
    if (preferDark === null) {
      // default to light
      preferDark = false;
    }

    // set preference for CSS
    document.documentElement.dataset.appliedMode =
      preferDark ? 'dark' : 'light';
  </script>
  <!-- the rest of head content -->
</head>
<body>
  <app-root></app-root>
</body>
</html>
```

### settings page
This page allows altering to and from light and dark mode the last change is stored in localstorage allowing for retrieval by the pre-init script.This local storage is managed by the settings service which also provides an observable to allow components which need to subscribe to acquire styles via an class. But also edits the applied mode to change the css variables.

## Angular Opinions
It works, and I know it, but the grass is always greener, so I am just going to learn HTMX for next time.

In all seriousness...

Angular is solid and I do think that services are intuitive although do pose some unique risks particularly with error tracking as many components will likely use that service.

Does your content need to be updated on the server only, no the user can spare some cycles, but only some.
