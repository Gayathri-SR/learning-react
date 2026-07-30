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

**Performance Optimization**
**1. The core performance problem**
-> By default, when a component's state changes, React re-renders the components and recursively re-renders all it's children, even if those children didn't receive any new data. In large apps, this causes massive amounts of wasted CPU work
**2. Preventing unnecessary child re-renders**
**React.memo (Component caching)**
-> What it is : A higher-order component that wraps a functional component
-> How it works : It forces the child component to check it's incoming props. If the props haven't changed since the last render, the child skips re-rendering entirely
**The Synergy : React.memo + useCallback/useMemo**
-> If you pass an object/ a function as a prop to a component wrapped in 'React.memo', it will still re-render on every parent cycle because objects and functions get new memory references every render
-> To fix this, you must wrap the passed function in 'useCallback' or the passed object in 'useMemo'
**3. Fixing state-driven performance lags**
**A. State Co-location (Moving state down)**
-> Keep state as close to where it is used as possible. If only a single input field changes, don't put it's state at the top-level 'App' component: move it to a dedicated 'SearchInput' component so the whole app doesn't re-render on every keystroke
**B. Lazy initial state**
-> If your initial state requires an expensive calculation, pass a function instead of a raw-value to useState.
-> This ensures the heavy computation runs only once on mount, rather than on every render
**4. Optimizing large lists : Windowing/Virtualization**
-> The problem : Re-rendering thousands of DOM nodes for a massive list ruins the browser scrolling performance
-> The solution : Use list virtulization libraries (like 'react.window' or 'react.virtualized'). It ensures only the items currently visible inside the user's viewport are rendered in the DOM, dynamically swappimg them out as the user scrolls
**5. Improving initial load (Code splitting)**
-> The problem : SPA's bundle the entire app's JS into one massive file, causing slow initial page loads
-> The solution : Use 'React.lazy' and 'Suspense' to split your code into smaller chunks. The browser will only download a page's code when user actually navigates to it.

**Context API**
-> A built-in React feature that allows you to share global data(like themes, user authentication, or language settings) across the entire component tree without passing props manually through every level
-> Solves the problem - Prop drilling : the annoying process of passing props through 5 layers of intermediate components that don't actually need the data, just to get it to a deeply nested child
**The 3-Step Setup Blueprint**
**Step 1 : Create the context**
-> Create a context object using 'createContext'. This is your data warehouse
**Step 2 : Provide the context (At the top level)**
-> Wrap the parent component tree with the '.Provider' component. Pass the shared data into the 'value' prop.
**Step 3 : Consume the context (Deep in the tree)**
-> Use the 'useContext' hook inside any nested child component to instantly grab the data.
**Performance pitfall**
-> The issue : Whenever the 'value' inside a Context Provider changes, every single component that uses 'useContext' for that provider will instantly re-render, skipping 'React.memo'
-> The Rule of thumb :  Use context API for data that changes infrequently (Eg: Theme, user profile, language). Avoid it for high-frequency data updates(Eg : complex form tracking, game states, real-time streaming). For those, dedicated state managers like Redux/Zustand are preferred.

**Prop drilling vs Context API**
**Prop Drilling**
-> The manual process of passing data from a high-level parent component down through multiple layers of intermediate child components to reach a deeply nested component
**Context API** - Defined in prev. topic
**Structural comparision**
**Prop Drilling (The 'Pass-Along Approach)**
-> Data must travel sequentially through every layer, even if intermediate components don't use it.
-> Grandparent => Parent(ignores) => Child(ignores) => Deep child(uses)
**Context API (The 'Broadcast' Approach)**
-> Data is broadcasted globally. Components plug directly into the feed.
-> Grandparent (Provider) => Deep child (useContext)
**Trade-offs : Prop Drilling | Context API**
-> Setup overhead : None. Uses standard function parameters | Moderate. Creation of Context, provider and a hook.
-> Code cleanliness : Messy. Components get cluttered with props they don't use | Clean. removes boilerplate props from intermediate components
-> Component reusability : High. Components remain pure functions completely dependent on explicit inputs | Lower. Tightly couples the consuming child component to its specific Provider.
-> Performance tracking : Easy to track exactly where data is moving through the files | Harder. Re-renders every consuming component whenever context value changes
**When to use which**
-> Prop drilling : The data only needs to go 2-3 levels deep. It keeps the components decoupled, explicityly typed, and easy to test.
-> Context API : The data is truly global and needed by dozens of components scattered across different branches of the app (Eg : Current user theme, language settings, user authentication status)

**Scalable Architecture**
-> A structured codebase design that allows an app to grow cleanly in features, team size, and complexity without degenerating into unmaintainable "sphagetti code"
-> The goal : High Cohesion (related code stays together), Low coupling (components don't depend heavily on each other's internals).
**Industry standard directory structure : Feature-Based**
-> Instead of organizing files by technical type(eg. all components in one folder, all hooks in another), scale the app by 'domain feature'
-> The 'index.js' (public API) pattern : Inside each feature folder, use an 'index.js' file to explicitly export only what the rest of the app is allowed to use. This prevents other parts of the app from importing internal, private files of that feature.
**The 3 Architectural Global Rules**
**Rule 1 : Presentational vs Container Components**
-> Separate your UI look from your logic to make testing and refactoring instant:
    => Presentational (Dumb) : Accepts props and renders JSX. No network calls/complex calculations
    => Container (Smart) : Manages state, handles business logic, and maps data to presentation components.
**Rule 2 : Move business logic to custom hooks**
-> Never leave heavy data sorting, state manipulation, or API calling logic directly inside a UI layout component. Extract it into a custom hook.
**Rule 3 : Enforce strict dependency flow**
-> Shared components(/components) must never import anything from feature folders(/features). They must remain generic
-> Feature components can import from shared components or their own local sub-folders.
**Scalable Architecture tooling**
-> As you code at scale, rely on automatic tooling rather than human memory to maintain quality :
    => Linting & Formatting - ESLint & Prettier - Enforces strict, uniform code style and catches common code smells automatically
    => Type safety - TypeScript - Prevents runtime errors by catching invalid props or object shapes at compile time
    => Global state management - Zustand/Redux toolkit - Isolates volatile global data out of the React view layer completely
    => Data fetching & Cache - Tanstack Query (React Query) - Eliminates boilerplate 'useEffect' data-fetching loops with automatic caching & retry states.

**Reusable component**
-> A component designed to be used in multiple places across an app with different data, styles or behaviors
-> The Goal - Write code once, test it once, and reuse it everywhere to maintain consistency and slash code duplication
**The Anatomy of Reusability**
-> To make a component reusable, it must be 'parameterized' using Props. It should not hardcode any specific text, images or business logic
**Core Principles for Engineering Reusability**
**A. The Single Reusability Principle (SRP)**
-> A component should do exactly one thing visually.
-> A 'Card' component should only handle the layout container, border and padding. It shouldn't care if it's displaying a user, a product, or a weather update.
**B. Inversion of Control (Component Composition)**
-> Instead of adding endless layout props to a component, pass elements directly into it using the 'children' prop. This lets the parent decide what goes inside, keeping the child component generic.
**The Common Trap : Over-Engineering**
-> The Danger : Trying to make a component do everything by adding dozens of configuration props. This makes the file unmaintainable and hard to read.
-> The Rule of thumb : If a component requires more than 5-6 configuration props just to change it's look, it's better to break it into two separate, simpler components.

**Custom hooks - deep dive**
-> Key Rule : Custom hooks share stateful logic, not state itself. Two components using the same custom hook get completely isolated state.
**When to create a custom hook**
-> Create a custom hook when you notice :
    => Duplicated Effects : The same 'useEffect' or API-fetching logic appears in multiple components.
    => Bloated Components : A component file has 30+ lines of helper logic, state declarations, and event handlers obscuring the JSX
    => Complex Stateful Logic : complex UI patterns like tracking forms, window resizing, or local storage syncing.
**Architecture Blueprint**
-> It acts as a middleman between raw data/APIs and your view layout.
-> External System/Event => Custom Hook (State+Effect) => Clean Output (Data+Handlers) => UI Component (JSX).
**Return Patterns : Objects vs Arrays (Tuples)**
**Object Return**
-> Eg : return {data, loading, error};
-> Best for : Complex hooks returning multiple properties
-> Pros : Property order doesn't matter; caller can pick and choose what to destructure.
-> Cons : Requires explicit renaming if called twice ({data : user})
**Array Return**
-> Eg : return [value,setValue];
-> Best for : Simple hooks tracking a single value pair
-> Pros : Caller can easily pick any variable names on destructure.
-> Cons : Strict positional order required.
**Golden Rules for custom hooks**
-> Name must start with 'use' : Enforced by React's linter. This signals React that the function must follow the 'Rules of Hooks'
-> Never call hooks conditionally : The strict top-level execution rule applies inside custom hooks just as it does in standard components
-> Keep them pure & focused : One hook should handle one responsibility. Don't build a single 'useAppEverything' mega-hook.