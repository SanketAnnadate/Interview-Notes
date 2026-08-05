---
title: Rules of Hooks
tags:
  - react
  - hooks
  - interview
  - best-practices
created: 2026-08-03
---

# Rules of Hooks

> [!INFO]
> Hooks are special functions introduced in **React 16.8** that allow functional components to use React features like **state**, **lifecycle methods**, **context**, **refs**, and more.

> [!IMPORTANT]
> React Hooks **must always follow specific rules**. Breaking these rules can cause bugs, inconsistent state, and runtime errors.

---

# Why Do Hooks Have Rules?

React internally stores Hooks in the **order they are called**.

Example

```
Render 1

1. useState()

2. useEffect()

3. useRef()
```

Next render

```
Render 2

1. useState()

2. useEffect()

3. useRef()
```

The order must remain exactly the same.

If the order changes, React will associate the wrong state with the wrong Hook.

---

# Rule 1 — Only Call Hooks at the Top Level

> [!SUCCESS]
> Always call Hooks at the top level of your component.

✅ Correct

```jsx
function App() {

    const [count, setCount] = useState(0);

    const [name, setName] = useState("");

    useEffect(() => {
        console.log("Mounted");
    }, []);

    return <h1>{count}</h1>;
}
```

---

❌ Wrong

```jsx
function App() {

    if (true) {
        useState(0);
    }

    return <div>Hello</div>;
}
```

Problem

```
First Render

useState()

↓

Second Render

Condition False

↓

Hook Missing

↓

React gets confused
```

---

# Rule 2 — Never Call Hooks Inside Loops

❌ Wrong

```jsx
for (let i = 0; i < 5; i++) {
    useState(i);
}
```

Reason

Different loop iterations change the Hook order.

---

✅ Correct

```jsx
const [count, setCount] = useState(0);

const numbers = [1, 2, 3];
```

Loop only for rendering.

---

# Rule 3 — Never Call Hooks Inside Conditions

❌ Wrong

```jsx
if (isLoggedIn) {
    useEffect(() => {
        console.log("Logged In");
    }, []);
}
```

---

✅ Correct

```jsx
useEffect(() => {

    if (isLoggedIn) {
        console.log("Logged In");
    }

}, [isLoggedIn]);
```

Notice

Hook stays at the top level.

Only the logic is conditional.

---

# Rule 4 — Never Call Hooks Inside Nested Functions

❌ Wrong

```jsx
function App() {

    function loadData() {

        useState(0);

    }

}
```

---

✅ Correct

```jsx
function App() {

    const [count, setCount] = useState(0);

    function loadData() {

        console.log(count);

    }

}
```

---

# Rule 5 — Never Call Hooks Inside Event Handlers

❌ Wrong

```jsx
function App() {

    function handleClick() {

        useState(0);

    }

}
```

---

✅ Correct

```jsx
function App() {

    const [count, setCount] = useState(0);

    function handleClick() {
        setCount(count + 1);
    }

}
```

---

# Rule 6 — Never Call Hooks Inside try/catch

❌ Wrong

```jsx
try {

    useEffect(() => {

    }, []);

}
catch {

}
```

---

✅ Correct

```jsx
useEffect(() => {

    try {

        fetchData();

    } catch (e) {

        console.log(e);

    }

}, []);
```

---

# Rule 7 — Only Call Hooks from React Components

✅ Correct

```jsx
function Login() {

    const [user, setUser] = useState(null);

}
```

---

❌ Wrong

```jsx
function calculateTax() {

    useState(0);

}
```

Regular JavaScript functions cannot use Hooks.

---

# Rule 8 — Hooks Can Be Called from Custom Hooks

React allows Hooks inside custom Hooks.

Example

```jsx
function useCounter() {

    const [count, setCount] = useState(0);

    return { count, setCount };

}
```

Usage

```jsx
function App() {

    const { count } = useCounter();

}
```

---

# Internal Working

```
First Render

↓

useState()

↓

useEffect()

↓

useRef()

↓

React Stores

↓

[Hook1]

[Hook2]

[Hook3]
```

Second Render

```
React expects

↓

useState()

↓

useEffect()

↓

useRef()
```

Same order every time.

---

# What Happens If Order Changes?

First Render

```
1 useState

2 useEffect

3 useRef
```

Second Render

```
1 useState

2 useRef

3 useEffect
```

React now thinks

```
Hook 2

↓

Old useEffect

↓

New useRef
```

State becomes corrupted.

---

# Common Errors

## Invalid Hook Call

```
Invalid hook call.
Hooks can only be called inside the body of a function component.
```

Reason

Called Hook inside

- Function
- Loop
- Condition
- Class Component

---

## Rendered More Hooks Than Previous Render

Example

```jsx
if (isAdmin) {

    useState(0);

}
```

Sometimes the condition is true,

sometimes false.

Number of Hooks changes.

React throws an error.

---

# React's Hook Execution Order

```
Component Render

↓

useState

↓

useReducer

↓

useContext

↓

useRef

↓

useMemo

↓

useCallback

↓

useEffect

↓

Return JSX
```

Every render

Same order.

---

# Real-World Example

### Authentication

❌ Wrong

```jsx
if (loggedIn) {

    useEffect(() => {

        fetchProfile();

    }, []);

}
```

---

✅ Correct

```jsx
useEffect(() => {

    if (loggedIn) {

        fetchProfile();

    }

}, [loggedIn]);
```

---

# Best Practices

- Keep all Hooks at the top of the component.
- Never place Hooks inside loops or conditions.
- Put conditions **inside** Hooks, not around them.
- Create custom Hooks to reuse Hook logic.
- Use the React ESLint plugin (`eslint-plugin-react-hooks`) to catch mistakes automatically.

---

# Interview Questions

## Beginner

### What are the Rules of Hooks?

1. Only call Hooks at the top level.
2. Only call Hooks from React function components or custom Hooks.

---

### Why shouldn't Hooks be inside loops?

Because Hook order changes between renders.

---

### Why shouldn't Hooks be inside conditions?

React expects Hooks to be called in the same order every render.

---

## Intermediate

### Why does React care about Hook order?

React stores Hook state internally using the order in which Hooks are called. Changing the order causes React to associate state with the wrong Hook.

---

### Can Hooks be called inside custom Hooks?

Yes.

Custom Hooks are designed to encapsulate reusable Hook logic.

---

### Can Hooks be used in class components?

No.

Hooks work only in functional components and custom Hooks.

---

## Senior-Level Questions

### How does React identify different Hooks internally?

React uses the sequence of Hook calls during rendering. Each Hook call corresponds to a position in an internal linked list of Hook state.

---

### Why does React use call order instead of Hook names?

Using call order avoids runtime lookups and enables a simple, performant implementation. This design requires Hooks to be called consistently.

---

### What happens if a Hook is skipped because of a condition?

Subsequent Hooks shift positions, causing React to read and update the wrong state, leading to bugs or runtime errors.

---

# Scenario-Based Questions

## Scenario 1

You need to fetch user data only after login.

**Wrong**

```jsx
if (loggedIn) {
    useEffect(() => {
        fetchUser();
    }, []);
}
```

**Correct**

```jsx
useEffect(() => {
    if (loggedIn) {
        fetchUser();
    }
}, [loggedIn]);
```

---

## Scenario 2

You want to call `useState()` inside a button click.

**Answer**

Don't.

Declare the Hook at the top level and update it inside the click handler.

---

## Scenario 3

You need reusable state logic for multiple components.

**Answer**

Create a custom Hook.

```jsx
function useCounter() {
    const [count, setCount] = useState(0);
    return { count, setCount };
}
```

---

# Quick Revision

| Rule | Allowed? |
|--------|----------|
| Top-level Hooks | ✅ |
| Inside Function Component | ✅ |
| Inside Custom Hook | ✅ |
| Inside Condition | ❌ |
| Inside Loop | ❌ |
| Inside Event Handler | ❌ |
| Inside Nested Function | ❌ |
| Inside try/catch | ❌ |
| Inside Regular JS Function | ❌ |
| Inside Class Component | ❌ |

---

# Excalidraw Mind Map

```text
                    Rules of Hooks

                           │

        ┌──────────────────┼──────────────────┐

        │                  │                  │

   Top Level         Function Component   Custom Hook

        │                  │                  │

        └──────────────────┼──────────────────┘

                           │

                     Same Order

                           │

        ┌──────────────────┼──────────────────┐

        │                  │                  │

   No Conditions      No Loops         No Nested Functions

        │                  │                  │

        └──────────────────┼──────────────────┘

                           │

                  No Event Handlers

                           │

                  No try/catch Blocks

                           │

                    Predictable State

                           │

                   Reliable Re-renders
```

---

# Related Notes

- [[Hooks]]