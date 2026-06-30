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

**Functional components**
-> A plain JS function that returns a react element (JSX)
-> They replaced older, more complex Class components
**Syntax rules**
-> Must capitalize name (UserProfile)
-> Must return JSX
**Working**
1. Accepting Props:
    -> They accept a single object argument containing data from a parent
    -> JS destructuring can be used to pull values
2. Managing state (Hooks):
    -> Since they're just standard functions, they rely on special built-in React functions called 'Hooks' (like useState) to remember data between renders
**Uses**
-> Simpler code
-> Easier to read and test
-> Better performance

**Props**
-> Properties - a configuration mechanism used to pass data from a parent component to a child component
**Core characteristics**
-> Immutable by child components
-> Data flow is unidirectional (top->down)
**Special prop - children**
-> Every component automatically receives a prop called 'children'
-> It contains whatever content or nested components are placed between its opening and closing tags

**State**
-> A built-in object used to store data that is local and private to a component
-> When State changes, react automatically re-runs the component function and updates the UI(re-rendering)
**State rules**
-> Never modify state directly, as React won't know that the data changes and the UI will stay broken
-> State is asynchronous, hence can't read it immediately after being set
-> Always create a new copy using spread operator(...) and treat them as immutable
**useState**
-> A React hook that allows you to add State(local memory) to a functional component

**Event handling**
-> Capturing user interactions and executing JS code in response
-> React uses camelCase naming convention ('onClick instead of 'onclick') and passes a function reference directly instead of a string template literal
   (<button onClick={funcName}>BtnText</button>)
**Rules of react events**
-> Pass the function reference, don't call it
-> Pass arguments to event handlers via arrow functions
-> They have browser's native event in a cross-browser 'SyntheticEvent' wrapper (e)
**Common React events**
-> onClick, onChange, onSubmit

**Re-rendering**
-> The process where React calls a component a 2nd(or 3rd or 4th) time to figure out the visual changes in the UI based on new data
-> Flow : Data changes => Component re-runs => UI updates
**What triggers a re-render?**
-> State changes
-> Prop changes
-> Parent re-renders
**Render cycle**
**1. Render phase(Internal Math)**
-> React executes the component function and generates the new JSX tree, and compares ot with the old one inside the 'Virtual DOM'. This comparision process is called 'Diffing'
**2. Commit phase(DOM update)**
-> React takes the differences found during diffing and surgically updates the real browser DOM
-> Rule : If the render phase found no changes, this phase is skipped and the real DOM remains untouched
**Performance pitfall - Infinite loops**
-> An infinite render loop happens when State is updated directly inside the main body of a component
-> Fix : Always isolate state updates inside 'Event handlers' (like onClick)/'useEffect' hook with a proper dependency array

**Real DOM vs VDOM**
-> The real DOM treats updates with a heavy hand - frequently destroying and rebuilding larger layout blocks than necessary
-> The VDOM acts as a 'buffer layer', it batch-processes changes on a digital blueprint first, ensuring the real DOM is touched only where the data is changed

**Reconciliation**
-> The internal algorithm react uses to compare 2 VDOM trees (the old & the new one) to determine exactly which parts of the real DOM needs to be updated
-> The goal : To make UI updates as fast as possible by finding the min. no. of changes to sync the screen with the data
**Tree comparision and the diffing algorithm**
**The problem** - Comparing 2 tree structures node-by-node is very slow, costing O(n^3)
**The react solution** - A 'Heuristic diffing algorithm'  at O(n) by making 2 smart assumptions:
    1. 2 elements of different types will produce different trees
    2. Developers can provide a 'key' prop to hint which child elements are stable across renders
-> If the root tag of a UI element changes, React tears down the entire old tree and builds a brand-new one
-> If the HTML tags match but the attributes change, React keeps the DOM node and only updates the altered attributes
**Why react uses keys**
-> When rendering a list of synamic items, React needs a way to track which items were added, removed or rearranged.
-> Key attribute= unique ID
**Rules for keys**
-> Must be unique among sibling elements
-> Must be permanent and predictable
-> Avoid using array indexes as keys if the list can be filtered, sorted or reordered

**Hooks**
-> Special built-in functions that let you "hook into" React state and lifecycle features from functional components
-> Before hooks : Complex class components to manage state or lifecycle methods. Hooks made functional components fully capable
**Core rules of hooks**
-> React relies on the order in which Hooks are called. To prevent breaking this order, the following rules should be followed :
    => Only call hooks at the top level - Never call them inside loops, conditional statements or nested functions
    => Only call hooks from React functions - Call them only from React functional components or your own custom hooks. Do not call them from regular JS functions.
**The big three hooks**
**1.useState (State management)**
-> Allows a component to remember data between renders
-> Calling 'setState' doesn't just change a variable; it explicitly flags the component as "dirty" inside React's engine, scheduling a re-render
**2.useEffect (Side effects)**
-> Lets you synchronize your component with external systems (API data fetching, subscriptions, manual DOM manipulation)
**Dependency array**
-> It tells React exactly when to execute or skip your side effect function
-> React uses 'Object.is()' (Shallow comparision) to determine if a value in the DA has changed
**DA values and uses**
-> undefined (no array) : Runs on every single render - for general logging/data fetching
-> [] (empty array) : Runs exactly once when the component mounts - For API data fetching, event listeners
-> [prop,state] : Runs on mount, and then only if 'prop' or 'state' changes - for fetching new data when something changes
**DA working**
-> Primitives (Strings,Numbers,Booleans) : Checked by value
-> Objects and Arrays : Checked by reference, not content
**3.useRef (Persistence without rendering)**
-> Stores a mutable value that persists across renders, but does not trigger a re-render when it changes. Also used to reference real DOM nodes directly
-> Used for storing timer IDs, keeping track of previous state, or focusing an input element manually
**Advanced optimization hooks**
-> When your application grows, React provides hooks to cache(memoize) computations so that re-renders are fast
**1.useMemo**
-> Caches the result of a costly calculation so it doesn't re-run on every render unless its inputs change
**2.useCallback**
-> Caches the function definition itself. Useful when passing functions as props to optimized child components to prevent unnecessary re-renders
**Custom hooks**
-> A JS function whose name starts with 'use' and can call other hooks
-> To extract component logic into reusable, shareable functions
**Pitfalls of caching hooks**
**1. Overuse (Premature optimization)**
-> Mistake : Wrapping absolutely every simple function/math equation in 'useCallback' or 'useMemo'
-> Why it hurts : The hooks themselves have a performance overhead (React has to run DA comparisions on every render). For simple tasks, the overhead of the hook is more expensive than just recreating the function/variable
-> Rule of thumb : Don't use them until you hit a noticeable performance lag or are passing props to a heavily optimized child tree
**2. Stale closures**
-> Mistake : Forgetting to include a variable inside the DA
-> Why it hurts : The cached function/value will lock in the variable values from the 'very first render'. If those variables change later, the cached function will keep using the old, broken data.