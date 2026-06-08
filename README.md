# learning-react
Creating this repo to learn react the right way


**Problems with vanilla JS UI development**
-> vanilla JS is imperative - dev has to tell the browser 'exactly how' to change the UI step-by-step
-> Modern FRWKs use a declarative approach. Using states, UI is updated automatically when the data changes
-> In vanilla JS, as an app grows, 'querySelector', 'addEventListener', 'textContent' etc are present across a dozen files. If one update path is missed, UI becomes buggy and inconsistent with data
-> If 5 people work on one vanilla JS project, it will have 5 different architectural styles. Without pre-defined patterns, large codebases easily turn into unmaintainable "sphagetti" code
-> To avoid updating individual DOM elements, one might wipe out and  re-write an entire section.
    element.innerHTML = `<div class="user">${user.name}</div>`
    This forces the DOM to destory the DOM nodes & rewrite them which drops user focus, CSS transitions and is highly inefficient
-> FRWKS use 'virtual DOM' to find exactly what changed & update only those in the real DOM
-> If user.name=`<script>MaliciousCode()</script>`, it will get executed in vanilla JS.
-> FRWKS handle text insertion safely by default (automatically escape characters and treat them as plain text).

**Single Page Application (SPA)**
-> Rewrite the existing page locally using JS without a full reload like MPAs do
**Working**
-> The browser requests the site. The server sends back a single, mostly empty index.html along with a bundle of JS and CSS
-> THE JS FRWK mounts inside the HTML file and renders the initial UI
-> When data is needed, the app fetches raw JSON from a backend API (not new HTML pages)
-> The FRWK surgically updates the specific DOM elements based on incoming data
**Pros and Cons**
-> Provides desktop-like speed but heavy initial load
-> Rich UI transitions but SEO challenges as no HTML content initially
-> Reduced server load (assets are delivered only once) but memory management (leaks) as the app stays open indefinitely

**Component-Based Architecture**
-> Component - A self-contained, independent block of UI containing HTML/JSX, JS logic and CSS
-> One component=one responsibility
**Pros and Cons**
-> Highly reusable, easy to isolate/fix bugs, cleaner files
-> Over-splitting

**What is React**
-> An open-source library developed by Facebook used strictly for building UIs
**Core Pillars**
-> Component-based
-> Declarative
-> Virtual DOM

**JSX**
-> JS XML - A syntax extension to write HTML-like code directly into JS files
-> Visually combines layout and logic in a single file that helps see exactly how a component looks & behaves
**Working**
-> Browsers cannot read JSX directly. A build tool like Babel or Vite compiles it into stamdard JS

**Rendering elements**
-> An element is the smallest building block of a React app while a Component is a function that returns elements
**Process**
-> Every react app has a single root node where the entire app mounts
-> React uses 'createRoot' to target that root and render() method to display the element inside it
**Core rules**
-> Elements are immutable
-> Updates are performed only when & where necessary
-> Conditional remdering can be used inside JSX to choose which elements to render