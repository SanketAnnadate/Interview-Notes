---
title: React Hooks
tags:
  - react
  - hooks
  - useState
  - useEffect
  - useContext
  - useReducer
  - useRef
  - useMemo
  - useCallback
  - interview
created: 2026-08-02
---

# React Hooks

> [!INFO]
> **Hooks** are special functions introduced in **React 16.8** that allow **Function Components** to use React features such as state, lifecycle, context, and performance optimizations without writing class components.

> [!IMPORTANT]
> Hooks **cannot be called inside loops, conditions, or nested functions**. They must always be called at the **top level** of a React function component or a custom hook.

---

# Beginner Explanation

Before Hooks

```
Need State

↓

Use Class Component
```

After Hooks

```
Need State

↓

Use Function Component

↓

Hooks
```

Hooks made Function Components as powerful as Class Components.

---

# Why Hooks Were Introduced?

Before Hooks

```
Large Classes

↓

this keyword

↓

Lifecycle Methods

↓

Complex Code
```

After Hooks

```
Small Functions

↓

Reusable Logic

↓

Cleaner Code

↓

Better Performance
```

---

# React Hooks Overview

```text
                        React Hooks

                              │

      ┌───────────────────────┼────────────────────────┐

      │                       │                        │

   State Hooks           Effect Hooks           Performance

      │                       │                        │

 useState()             useEffect()           useMemo()

 useReducer()                                useCallback()

      │

      ├──────────────┐

      │              │

 useContext()      useRef()
```

---

# 1. useState()

## Purpose

Stores and updates local component state.

---

### Syntax

```jsx
const [

    state,

    setState

] = useState(initialValue);
```

---

### Example

```jsx
import {

    useState

} from "react";

function Counter(){

    const [

        count,

        setCount

    ] = useState(0);

    return(

        <button

            onClick={()=>

                setCount(

                    prev => prev + 1

                )

            }

        >

            {count}

        </button>

    );

}
```

---

### When to Use?

- Counter
- Toggle
- Forms
- Input Fields
- Small UI State

---

### Interview Question

**When should you use `useState`?**

For simple component-level state.

---

# 2. useEffect()

## Purpose

Performs **side effects**.

Examples

- API Calls
- Timers
- Event Listeners
- WebSockets
- Local Storage

---

### Syntax

```jsx
useEffect(()=>{

    fetchData();

},[]);
```

---

### Example

```jsx
useEffect(()=>{

    console.log(

        "Mounted"

    );

},[]);
```

---

### Dependency Array

```jsx
[]
```

Runs only once.

---

```jsx
[count]
```

Runs when count changes.

---

```jsx
(no dependency array)
```

Runs after every render.

---

### Cleanup

```jsx
useEffect(()=>{

    const timer=

        setInterval(update,1000);

    return ()=>{

        clearInterval(timer);

    };

},[]);
```

---

### Interview Question

**Why use `useEffect` instead of calling APIs directly in the component body?**

Because the component function executes on every render, while `useEffect` lets you control when side effects occur.

---

# 3. useContext()

## Purpose

Share data across components without **prop drilling**.

---

Without Context

```text
App

↓

Header

↓

Navbar

↓

Profile

↓

User
```

Props passed through every component.

---

With Context

```text
App

↓

Context

↓

Any Component
```

---

### Example

```jsx
const UserContext=

createContext();

function App(){

return(

<UserContext.Provider

value="John">

<Home/>

</UserContext.Provider>

);

}
```

Read

```jsx
const user=

useContext(UserContext);
```

---

### Use Cases

- Theme
- Logged-in User
- Language
- Permissions

---

# 4. useReducer()

## Purpose

Manages **complex state logic**.

Think of it as a small Redux.

---

### Flow

```text
Action

↓

Reducer

↓

New State
```

---

### Example

```jsx
const reducer=(

state,

action

)=>{

switch(action.type){

case "INC":

return{

count:

state.count+1

};

default:

return state;

}

};

const[

state,

dispatch

]=useReducer(

reducer,

{

count:0

});
```

---

### When to Use?

- Shopping Cart
- Forms
- Dashboards
- Complex Objects

---

### Interview Question

**Difference between `useState` and `useReducer`?**

| useState | useReducer |
|----------|------------|
| Simple State | Complex State |
| Easy | Better for multiple related updates |
| Less Boilerplate | Centralized logic |

---

# 5. useRef()

## Purpose

Stores a mutable value **without causing re-renders**.

Also used for accessing DOM elements.

---

### DOM Example

```jsx
const inputRef=

useRef();

<input

ref={inputRef}

/>
```

---

### Mutable Value Example

```jsx
const renderCount=

useRef(0);

renderCount.current++;
```

---

### Use Cases

- Focus Input
- Timers
- Previous Values
- DOM Access

---

# 6. useMemo()

## Purpose

Caches expensive calculations.

---

Without Memo

```text
Render

↓

Expensive Calculation

↓

Render Again

↓

Expensive Calculation Again
```

---

With Memo

```text
Render

↓

Calculation

↓

Stored

↓

Reuse Cached Value
```

---

### Example

```jsx
const total=

useMemo(

()=>{

return

calculate();

},

[data]

);
```

---

### Use Cases

- Sorting
- Filtering
- Large Tables
- Heavy Calculations

---

# 7. useCallback()

## Purpose

Caches function references.

---

Without Callback

```
Render

↓

New Function

↓

Child Re-renders
```

---

With Callback

```
Render

↓

Same Function

↓

Child Doesn't Re-render
```

---

### Example

```jsx
const save=

useCallback(

()=>{

saveData();

},

[]

);
```

---

### Use Cases

- Passing callbacks to child components
- React.memo optimization
- Large lists

---

# Difference Between useMemo & useCallback

| useMemo | useCallback |
|----------|-------------|
| Caches values | Caches functions |
| Returns value | Returns function |
| Expensive calculations | Prevent unnecessary child renders |

---

# Other Important Hooks

| Hook | Purpose |
|------|---------|
| useLayoutEffect | Runs synchronously after DOM updates but before the browser paints |
| useImperativeHandle | Customize the instance value exposed through a ref |
| useDeferredValue | Defer updating non-urgent values |
| useTransition | Mark state updates as non-urgent for smoother UI |
| useId | Generate stable unique IDs |
| useSyncExternalStore | Subscribe to external stores safely |
| useInsertionEffect | Used mainly by CSS-in-JS libraries before DOM mutations |

---

# Rules of Hooks

✅ Call Hooks only at the top level.

✅ Call Hooks only inside React Function Components or Custom Hooks.

❌ Never call Hooks inside:

- Loops
- Conditions
- Nested functions

Wrong

```jsx
if(

loggedIn

){

useEffect(()=>{});

}
```

---

# Internal Working

```text
Component Render

↓

Hooks Execute

↓

State Stored

↓

Virtual DOM

↓

Reconciliation

↓

Commit Phase

↓

Browser Paint
```

---

# Hook Selection Guide

| Situation | Hook |
|-----------|------|
| Counter | useState |
| API Call | useEffect |
| Theme | useContext |
| Shopping Cart | useReducer |
| Focus Input | useRef |
| Expensive Calculation | useMemo |
| Prevent Child Re-render | useCallback |

---

# Common Mistakes

## Calling Hook Inside if

Wrong

```jsx
if(

isAdmin

){

useState();

}
```

---

## Forgetting Dependency Array

Wrong

```jsx
useEffect(()=>{

fetchUsers();

});
```

Runs after every render.

---

## Using useMemo Everywhere

Don't.

Only use it for expensive calculations.

---

## Using useCallback Everywhere

Don't.

Use it only when function identity matters (e.g., with `React.memo` or dependency arrays).

---

# Interview Questions

## Beginner

### What are Hooks?

Special functions that let Function Components use React features like state and lifecycle behavior.

---

### Which Hook manages state?

```jsx
useState()
```

---

### Which Hook performs API calls?

```jsx
useEffect()
```

---

### Which Hook accesses DOM elements?

```jsx
useRef()
```

---

## Intermediate

### Difference between useState and useReducer?

`useState` is ideal for simple state.

`useReducer` is better for complex state transitions involving multiple related updates.

---

### Difference between useMemo and useCallback?

`useMemo` caches **values**.

`useCallback` caches **functions**.

---

### Difference between useRef and useState?

| useRef | useState |
|--------|----------|
| Doesn't trigger re-render | Triggers re-render |
| Mutable | Reactive |
| Stores references or mutable values | Stores UI state |

---

# Senior-Level Interview Questions

### Explain the lifecycle of a function component using Hooks.

```text
Function Executes

↓

Render

↓

Commit Phase

↓

useEffect

↓

State Update

↓

Re-render

↓

Cleanup

↓

Unmount
```

---

### When should you avoid `useMemo`?

Avoid it for cheap calculations.

Memoization itself has a cost. Use it only when the computation or re-render savings outweigh the overhead.

---

### Why can excessive use of `useCallback` reduce performance?

Because React must store and compare memoized callbacks. If child components aren't memoized or function identity doesn't matter, `useCallback` adds complexity with little benefit.

---

# Scenario-Based Questions

## Scenario 1

A child component keeps re-rendering because its callback prop changes every render.

**Solution**

Use `useCallback` with `React.memo` where appropriate.

---

## Scenario 2

A table containing 100,000 rows becomes slow after sorting.

**Solution**

Memoize the sorted data using `useMemo`.

---

## Scenario 3

Your application needs to share the logged-in user across many components.

**Solution**

Use `useContext`.

---

## Scenario 4

A shopping cart has many actions (add, remove, update quantity).

**Solution**

Use `useReducer` because the state logic is complex and centralized.

---

# Quick Revision

| Hook | Purpose |
|------|---------|
| useState | Local State |
| useEffect | Side Effects |
| useContext | Global Shared Data |
| useReducer | Complex State |
| useRef | DOM / Mutable Values |
| useMemo | Cache Values |
| useCallback | Cache Functions |
| useLayoutEffect | DOM Measurements |
| useTransition | Non-urgent Updates |
| useDeferredValue | Defer Expensive Rendering |
| useId | Stable IDs |

---

# Excalidraw Diagram

```text
                        React Hooks

                              │

        ┌─────────────────────┼─────────────────────┐

        │                     │                     │

     State              Side Effects         Performance

        │                     │                     │

   useState()           useEffect()          useMemo()

   useReducer()                              useCallback()

        │

        ├───────────────┬──────────────────────┐

        │               │                      │

   useContext()     useRef()          Advanced Hooks

                                            │

          ┌─────────────────────────────────┼──────────────────────┐

          │                 │               │                      │

   useLayoutEffect()   useTransition()  useDeferredValue()   useId()
```

---

# Related Notes

- 