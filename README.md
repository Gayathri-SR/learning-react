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