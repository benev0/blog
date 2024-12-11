# this.blogTitle

Meta, I know, but who does not like a bit of meta humor. My personal experience with angular and web originated in Collage. During the duration of the class, I was not the most interested in Web, but ultimately I found that front end web was the only area where I could create a full project without too much hassle.

### Purpose of This Post
Describe the content and methods of [this webpage](https://benev0.github.io/). This page is not designed to be used as template or an example to create other pages, but there will be some demonstrations of the problems and solutions that this page faced. This post expects some understanding of HTML, CSS, and JS.

*HTML (Hyper Text Markup Language)* - a system of formatting data to be rendered to the page by the browser

*CSS (Cascading Style Sheets)* - a system of manipulating how HTML is rendered

*JS (Java Script)* - Run code in the browser to manipulate HTML and CSS

## Angular
Angular is google's answer to dissatisfaction within web dev. Angular allows for web content to be handled right in front of you instead of on a distant server. Is that a good thing? No, it is strictly unneeded for most things. There of course is the rare case where it may be good to not have to contact the server every time you make a change. These cases are in browser editors such as Word and google Docs. This conclusion is mostly due to the fact that users may not be connected to the internet at all times.

### Components
Components are a bundling of three things TypeScript (TS) which is like JS but with types, HTML templates which directly interface with the typescript, and CSS or a CSS variant because default HTML is always ugly. A component also has a selector which is the name that can be used like a standard html tag in other components' template HTML. Most of the work to create a component besides the copious boiler plate is the TS functionality. This is defined in a TS class the (public) methods and fields of this class may be used within the template HTML.

{diagrams coming soon}

### Services
Services differ from components in responsibility. Services manage information that needs to be accessed by a diverse and disparate set of components. Services follow the dependency injection pattern and are singleton by default. Services can be wrappers for global state or a collection of functions that are needed across the app like cotacting an API. A service's lifetime matches the lifetime of the page by default, and is accessible from every component that the service is injected into.

{diagrams coming soon}

### Router
This is simply a special component which changes which component it points to based off of the page route (text in the url). This can all be done without reloading the page like a normal link.

{diagrams coming soon}

## this site's design

The core elements of this site are:

0. the info page
1. the router and nav bar
2. the blog and blog navigation
3. the service to fetch the blogs
4. the pre-settings init process
5. the settings page & service

### info
As simple as they come this is a component that just hosts HTML content. Info is the default landing for the entry point of the site.

### nav bar
This is the primary control for this site. The nav bar is a simple display of routerlinks. Each one when clicked replacing the router component with the specified component allowing users to navigate the site.

### blog
The blog component has two parts the blog picker and the content. If the subroute points to a valid blog then the content for that blog is displayed otherwise a 404 is displayed. If there is no subroute a blog list is displayed. The blog content is a list of available blogs must be requested from the blog service. This service returns a promise which on completion updates the page with the requested content manifest or blog. The same service is used for any valid blog.

### blog service
This service returns promises for the blog manifest and blogs. If a blog or the blog manifest has been requested it is also cached. The blogs and blog manifest are fetched from a github repo via the raw file api. This is done via a Http client which runs in the browser.

### pre-init
This is a small script which indicates which version of the CSS should be used to select either light or dark mode. The script checks the local storage for any stored settings then, checks if the system (browser/os) has a preference otherwise the system will default to light mode. This is done to avoid any flicker on the user reloading the page by happening before any html is rendered. The pre-init script exists in the head of the base HTML and runs before any html is loaded thus preventing any style changes that would occur if this logic was ran in a component which is loaded with HTML.

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

This css is also required to utilize the pre-init script
```css
:root[data-applied-mode="light"] {
  /* light theme styles here */
}

:root[data-applied-mode="dark"] {
  /* dark theme styles here */
}
```

### settings page
This page allows selection of light and dark modes the last change is stored in localstorage allowing for retrieval by the pre-init script. This local storage is managed by the settings service which also provides an observable to allow components which need to subscribe to acquire styles which are not properly expressed without JS. The service also sets the appliedMode as seen before which is used on page reload to persist the settings.

## Thankyou for reading
that is all.
