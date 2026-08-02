# What are the Major Features of React?

> [!INFO] Definition
> React provides several powerful features that make it one of the most popular JavaScript libraries for building modern, fast, and scalable user interfaces.

---

# 1. Virtual DOM

## What is Virtual DOM?

The **Virtual DOM (VDOM)** is a lightweight copy of the Real DOM maintained in memory by React.

Instead of updating the browser's DOM directly, React:

1. Creates a Virtual DOM.
2. Compares the new Virtual DOM with the previous one (Diffing).
3. Updates only the changed elements in the Real DOM.

This process is called **Reconciliation**.

### Advantages

- Faster UI updates
- Better performance
- Minimal DOM manipulation
- Improved user experience

### Example

```jsx
function App() {
    const [count, setCount] = useState(0);

    return (
        <>
            <h1>{count}</h1>
            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </>
    );
}
```

When the button is clicked, React updates **only** the `<h1>` element instead of reloading the entire page.

---

# 2. JSX (JavaScript XML)

## What is JSX?

JSX is a syntax extension for JavaScript that allows developers to write HTML-like code inside JavaScript.

React converts JSX into JavaScript using Babel.

### JSX

```jsx
const element = <h1>Hello React</h1>;
```

### Converted JavaScript

```javascript
const element = React.createElement(
    "h1",
    null,
    "Hello React"
);
```

### Advantages

- Easier to read
- Easier to write
- Less boilerplate code
- Better developer experience

---

# 3. Component-Based Architecture

## What are Components?

Components are reusable building blocks of a React application.

Instead of writing one huge HTML page, React divides the UI into smaller independent components.

Example:

```
App

├── Navbar
├── Sidebar
├── Dashboard
├── Profile
├── Footer
```

Each component manages its own logic and UI.

### Advantages

- Reusability
- Maintainability
- Easy Testing
- Independent Development
- Better Code Organization

---

# 4. One-Way Data Binding (Unidirectional Data Flow)

React follows **One-Way Data Flow**, meaning data moves from **Parent → Child** through props.

```
Parent Component
        │
        ▼
 Child Component
```

### Advantages

- Predictable state
- Easier debugging
- Better control over application state
- Simpler data management

---

# 5. High Performance

React is highly performant because it uses:

- Virtual DOM
- Efficient Diffing Algorithm
- Reconciliation
- Fiber Architecture
- Batched Updates

These techniques reduce unnecessary rendering and improve application speed.

---

# 6. Declarative UI

React lets developers describe **what** the UI should look like rather than **how** to update it.

Instead of manually manipulating the DOM:

```javascript
document.getElementById("title").innerHTML = "Hello";
```

React automatically updates the UI:

```jsx
<h1>{title}</h1>
```

---

# 7. Reusable Components

A component can be used multiple times with different data.

Example:

```jsx
<UserCard name="John" />

<UserCard name="David" />

<UserCard name="Emma" />
```

This reduces code duplication and improves maintainability.

---

# 8. Hooks

Hooks allow functional components to use React features such as:

- State
- Lifecycle methods
- Context
- Refs
- Performance optimization

Common Hooks:

- useState
- useEffect
- useMemo
- useCallback
- useRef
- useReducer
- useContext

---

# 9. Fast Rendering

React updates only the modified components instead of re-rendering the entire page.

This results in:

- Faster rendering
- Better responsiveness
- Improved scalability

---

# 10. Cross-Platform Development

React knowledge can be extended to build:

- Web Applications (React)
- Mobile Applications (React Native)

---

# 11. Rich Ecosystem

React has a large ecosystem of libraries.

Examples:

- React Router
- Redux Toolkit
- React Query
- Zustand
- Material UI
- Axios
- React Hook Form

---

# 12. SEO Support

Using frameworks like **Next.js**, React supports:

- Server-Side Rendering (SSR)
- Static Site Generation (SSG)
- Incremental Static Regeneration (ISR)

This improves SEO and page load performance.

---

# Interview Questions

## Basic

### What are the major features of React?

**Answer:**

- Virtual DOM
- JSX
- Component-Based Architecture
- One-Way Data Flow
- High Performance
- Declarative UI
- Reusable Components
- Hooks
- Fast Rendering
- Rich Ecosystem
- Cross-Platform Development
- SEO Support with Next.js

---

## Intermediate

### Why is Virtual DOM faster than Real DOM?

Because React compares the previous and current Virtual DOM using the Diffing Algorithm and updates only the changed nodes in the Real DOM, reducing expensive DOM operations.

---

### Why does React use One-Way Data Flow?

It ensures predictable state management, simplifies debugging, and makes applications easier to maintain.

---

### Why are Components important?

Components promote code reusability, modularity, maintainability, and easier testing.

---

# Scenario-Based Questions

### Scenario 1

A dashboard contains 100 widgets. Updating one widget causes all widgets to re-render.

**How would you optimize it?**

**Expected Answer:**

- Use `React.memo`
- Use `useMemo`
- Use `useCallback`
- Split components
- Avoid unnecessary parent renders
- Use stable props

---

### Scenario 2

A table displays 50,000 records and scrolling becomes slow.

**How would you improve performance?**

**Expected Answer:**

- Virtualization (`react-window` / `react-virtualized`)
- Pagination
- Lazy loading
- Memoization
- Infinite scrolling

---

### Scenario 3

A child component unexpectedly changes parent data.

**What React principle prevents this?**

**Answer:**

React's **One-Way Data Flow**. Child components cannot directly modify parent state; they communicate changes via callback functions passed as props.

---

# Best Practices

- Build small reusable components.
- Keep components focused on a single responsibility.
- Use memoization only when needed.
- Avoid unnecessary state.
- Prefer composition over inheritance.
- Keep state as close as possible to where it is used.
- Use immutable updates.

---

# Quick Revision

| Feature | Purpose |
|---------|---------|
| Virtual DOM | Faster rendering |
| JSX | HTML-like syntax in JavaScript |
| Components | Reusable UI building blocks |
| One-Way Data Flow | Predictable state management |
| High Performance | Efficient DOM updates |
| Declarative UI | Simpler UI development |
| Hooks | State and lifecycle in functional components |
| Rich Ecosystem | Routing, state management, UI libraries |
| Cross-Platform | React + React Native |
| SEO Support | SSR/SSG with Next.js |

---

# Related Notes

- [[Virtual DOM]]
- [[JSX]]
- [[Components]]
- [[Props]]
- [[State]]
- [[Hooks]]
- [[React Fiber]]
- [[Reconciliation]]
- [[Performance Optimization]]
- [[export and import]]
```