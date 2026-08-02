---
title: React Component Lifecycle
tags:
  - react
  - lifecycle
  - hooks
  - useeffect
  - interview
  - rendering
created: 2026-08-02
---

# React Component Lifecycle

> [!INFO]
> The **React Component Lifecycle** describes the different stages a component goes through from the time it is created until it is removed from the DOM.

> [!IMPORTANT]
> Every React component goes through **three major phases**:
>
> 1. Mounting
> 2. Updating
> 3. Unmounting
>
> In modern React (Function Components), lifecycle behavior is managed primarily with **Hooks**, especially `useEffect`.

---

# Beginner Explanation

Imagine a person.

```
Birth

↓

Life

↓

Death
```

Similarly, a React component has a life.

```
Component Created

↓

Component Updated

↓

Component Removed
```

This is called the **Component Lifecycle**.

---

# React Lifecycle Overview

```text
                 React Component

                       │

      ┌────────────────┼────────────────┐

      │                │                │

  Mounting         Updating        Unmounting

      │                │                │

Created         State/Props       Removed

      │            Changes         from DOM
```

---

# Phase 1 — Mounting

A component is **created and inserted into the DOM**.

```
Component Created

↓

Render

↓

DOM Updated

↓

Visible to User
```

---

## Class Component Lifecycle

```jsx
constructor()

↓

render()

↓

componentDidMount()
```

---

## Functional Component

```jsx
useEffect(() => {

    console.log("Mounted");

}, []);
```

The empty dependency array (`[]`) means the effect runs only once after the component is mounted.

---

## Real-World Example

When a Dashboard opens:

- Fetch user data
- Load notifications
- Open WebSocket connection

All these tasks are commonly started after mounting.

---

# Phase 2 — Updating

Whenever **state** or **props** change, React updates the component.

```
State Changes

↓

Re-render

↓

Virtual DOM

↓

Reconciliation

↓

Updated UI
```

---

## Class Component

```jsx
componentDidUpdate(

    prevProps,

    prevState

){

    console.log("Updated");

}
```

---

## Functional Component

```jsx
useEffect(() => {

    console.log("Count changed");

}, [count]);
```

This effect runs whenever `count` changes.

---

## Example

```jsx
const [

    count,

    setCount

] = useState(0);
```

Click button

```
0

↓

1

↓

2

↓

3
```

Each update causes the component to re-render.

---

# Phase 3 — Unmounting

A component is removed from the DOM.

```
Navigate Away

↓

Component Removed

↓

Cleanup

↓

Memory Released
```

---

## Class Component

```jsx
componentWillUnmount(){

    console.log("Cleanup");

}
```

---

## Functional Component

```jsx
useEffect(() => {

    console.log("Mounted");

    return () => {

        console.log("Unmounted");

    };

}, []);
```

The returned function is called during cleanup.

---

# Complete Lifecycle

```text
Component Created

↓

constructor()

↓

render()

↓

componentDidMount()

↓

User Interaction

↓

setState()

↓

render()

↓

componentDidUpdate()

↓

User Leaves Page

↓

componentWillUnmount()
```

---

# Functional Component Lifecycle

```text
Component Created

↓

Function Executes

↓

Render

↓

useEffect()

↓

State Changes

↓

Render Again

↓

useEffect()

↓

Cleanup

↓

Component Removed
```

---

# Lifecycle in Modern React

| Class Component | Functional Component |
|-----------------|----------------------|
| constructor | Function Execution |
| componentDidMount | useEffect(..., []) |
| componentDidUpdate | useEffect(..., [dependencies]) |
| componentWillUnmount | Cleanup Function |

---

# Real-World Example

## Chat Application

Mount

```
Connect to Server
```

Update

```
Receive New Messages
```

Unmount

```
Disconnect WebSocket
```

---

## API Call

Mount

```jsx
useEffect(() => {

    fetchUsers();

}, []);
```

---

## Timer

```jsx
useEffect(() => {

    const timer =

        setInterval(

            updateClock,

            1000

        );

    return () => {

        clearInterval(timer);

    };

}, []);
```

Without cleanup,

timers continue running even after the component is removed.

---

# Internal Working

```text
Component Mount

↓

Render Phase

↓

Virtual DOM

↓

Commit Phase

↓

Browser Paint

↓

useEffect Runs
```

State Update

```text
setState()

↓

Scheduler

↓

Render Phase

↓

Virtual DOM

↓

Diffing

↓

Commit Phase

↓

Browser Paint

↓

useEffect Runs
```

Unmount

```text
Cleanup Function

↓

Component Removed

↓

Memory Released
```

---

# Common Mistakes

## Missing Cleanup

Wrong

```jsx
useEffect(() => {

    setInterval(

        update,

        1000

    );

}, []);
```

The interval keeps running after unmount.

---

Correct

```jsx
useEffect(() => {

    const timer =

        setInterval(

            update,

            1000

        );

    return () =>

        clearInterval(timer);

}, []);
```

---

## Infinite Loop

Wrong

```jsx
useEffect(() => {

    setCount(

        count + 1

    );

});
```

No dependency array.

The effect runs after every render, causing another state update and another render.

---

Correct

```jsx
useEffect(() => {

    setCount(

        count + 1

    );

}, []);
```

or use an appropriate dependency list.

---

# Best Practices

- Use `useEffect` for side effects.
- Always clean up timers, subscriptions, and event listeners.
- Keep effects focused on a single responsibility.
- Use dependency arrays correctly.
- Avoid unnecessary state updates inside effects.

---

# Interview Questions

## Beginner

### What is the React Lifecycle?

The lifecycle is the sequence of stages a component goes through from creation to removal.

---

### What are the three lifecycle phases?

- Mounting
- Updating
- Unmounting

---

### Which hook replaces lifecycle methods?

`useEffect`

---

# Intermediate

### Which lifecycle method is used for API calls?

Class Component

```jsx
componentDidMount()
```

Function Component

```jsx
useEffect(() => {

    fetchData();

}, []);
```

---

### What is the cleanup function?

The function returned from `useEffect`. It runs before the component unmounts and also before the effect re-runs when dependencies change.

---

### Why do we clean up effects?

To avoid memory leaks, duplicate subscriptions, and unnecessary background work.

---

# Senior-Level Interview Questions

### Explain the lifecycle of a Function Component.

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

Cleanup (if needed)

↓

useEffect Again

↓

Component Unmount

↓

Final Cleanup
```

---

### When exactly does `useEffect` run?

`useEffect` runs **after** React has committed updates to the DOM. Cleanup runs before the next effect execution (if dependencies changed) and when the component unmounts.

---

### Why shouldn't you fetch data directly inside the component body?

The component function runs on every render.

Fetching data inside the function body would execute on every render.

`useEffect` controls when side effects occur.

---

# Scenario-Based Questions

## Scenario 1

A timer continues running after leaving the page.

Why?

**Answer**

The interval wasn't cleared in the cleanup function.

---

## Scenario 2

An interviewer asks:

**"Why is my API called multiple times?"**

**Answer**

Possible reasons include:

- Missing or incorrect dependency array.
- State updates causing repeated effect execution.
- React Strict Mode (development only) intentionally re-running effects to detect side effects.

---

## Scenario 3

A WebSocket connection remains active after navigating away.

How do you fix it?

```jsx
useEffect(() => {

    socket.connect();

    return () => {

        socket.disconnect();

    };

}, []);
```

---

# Quick Revision

| Phase | Purpose | Class Component | Function Component |
|--------|---------|-----------------|-------------------|
| Mounting | Create Component | `componentDidMount()` | `useEffect(..., [])` |
| Updating | State/Props Change | `componentDidUpdate()` | `useEffect(..., [deps])` |
| Unmounting | Remove Component | `componentWillUnmount()` | Cleanup Function |

---

# Excalidraw Diagram

```text
                     React Lifecycle

                           │

        ┌──────────────────┼──────────────────┐

        │                  │                  │

    Mounting           Updating         Unmounting

        │                  │                  │

  Render Component     State Changes      Cleanup

        │                  │                  │

 componentDidMount     Re-render      componentWillUnmount

        │                  │                  │

 useEffect([])     useEffect([deps])    return () => {}

        │                  │                  │

        └──────────────────┼──────────────────┘

                     React Scheduler

                           │

                      Virtual DOM

                           │

                    Reconciliation

                           │

                       Updated UI
```

---

# Related Notes

- [[Components]]
- [[Hooks]]
- [[useEffect]]
- [[useState]]
- [[State]]
- [[Rendering]]
- [[Virtual DOM]]
- [[Reconciliation]]
- [[React Fiber]]
- [[Performance Optimization]]
- [[Synthetic Events]]