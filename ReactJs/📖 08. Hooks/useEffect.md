---
title: useEffect Hook
tags:
  - react
  - hooks
  - useeffect
  - lifecycle
  - side-effects
  - interview
created: 2026-08-02
---

# What is useEffect?

> [!INFO]
> `useEffect()` is a **React Hook** that allows Function Components to perform **side effects** after rendering.

> [!IMPORTANT]
> A **side effect** is any operation that interacts with something **outside the React rendering process**, such as:
>
> - API Calls
> - Timers
> - Event Listeners
> - Local Storage
> - WebSocket Connections
> - Updating the Document Title
> - Third-party Libraries
> - DOM Manipulation

---

# Beginner Explanation

Imagine a restaurant.

```
Customer Orders Food

↓

Kitchen Cooks Food

↓

Food Delivered
```

The cooking process happens **after** the order.

Similarly,

```
React Renders UI

↓

useEffect Executes

↓

Side Effect Happens
```

React first updates the screen, then executes `useEffect`.

---

# What is a Side Effect?

Anything that happens **outside rendering**.

Rendering should only describe the UI.

Everything else belongs in `useEffect`.

Examples

✅ Fetch API

✅ Save to Local Storage

✅ Start Timer

✅ Add Event Listener

✅ WebSocket Connection

---

# Before Hooks

Class Components

```jsx
componentDidMount(){}

componentDidUpdate(){}

componentWillUnmount(){}
```

---

# After Hooks

Function Components

```jsx
useEffect(()=>{

},[]);
```

Much simpler.

---

# Syntax

```jsx
useEffect(

()=>{

// Side Effect

},[dependencies]

);
```

---

# Basic Example

```jsx
import {

useEffect

} from "react";

function App(){

useEffect(()=>{

console.log(

"Component Mounted"

);

},[]);

return<h1>Hello</h1>;

}
```

Output

```
Component Mounted
```

Runs only once.

---

# Dependency Array

The dependency array controls **when** the effect runs.

---

## 1. Empty Dependency Array

```jsx
useEffect(()=>{

fetchUsers();

},[]);
```

Runs

```
Only Once

↓

After Initial Render
```

Best for

- API Calls
- Initial Setup

---

## 2. Dependency Array

```jsx
useEffect(()=>{

console.log(

count

);

},[count]);
```

Runs whenever

```
count

↓

Changes
```

---

## 3. No Dependency Array

```jsx
useEffect(()=>{

console.log(

"Runs"

);

});
```

Runs

```
Every Render
```

Usually not what you want.

---

# Cleanup Function

Some side effects must be cleaned up.

Example

```jsx
useEffect(()=>{

const timer=

setInterval(

updateClock,

1000

);

return()=>{

clearInterval(timer);

};

},[]);
```

Cleanup prevents

- Memory Leaks
- Duplicate Timers
- Duplicate Event Listeners
- Open WebSockets

---

# Visual Lifecycle

```text
Component Mount

↓

Render

↓

useEffect()

↓

State Update

↓

Re-render

↓

Cleanup (Old Effect)

↓

Run New Effect

↓

Component Unmount

↓

Final Cleanup
```

---

# API Call Example

```jsx
import {

useEffect,

useState

} from "react";

function Users(){

const[

users,

setUsers

]=useState([]);

useEffect(()=>{

fetch(

"https://api.example.com/users"

)

.then(res=>res.json())

.then(data=>

setUsers(data)

);

},[]);

return(

<div>

{users.length}

</div>

);

}
```

---

# Document Title

```jsx
useEffect(()=>{

document.title=

`Count ${count}`;

},[count]);
```

Every count change updates the browser title.

---

# Event Listener

```jsx
useEffect(()=>{

function resize(){

console.log(

window.innerWidth

);

}

window.addEventListener(

"resize",

resize

);

return()=>{

window.removeEventListener(

"resize",

resize

);

};

},[]);
```

---

# Local Storage

```jsx
useEffect(()=>{

localStorage.setItem(

"theme",

theme

);

},[theme]);
```

---

# WebSocket Example

```jsx
useEffect(()=>{

socket.connect();

return()=>{

socket.disconnect();

};

},[]);
```

---

# Internal Working

```text
Component Render

↓

Render Phase

↓

Virtual DOM

↓

Commit Phase

↓

Browser Paint

↓

useEffect Executes
```

Notice:

**useEffect always runs after the DOM has been updated and painted.**

---

# Dependency Array Flow

## Empty Array

```text
Mount

↓

Run Effect

↓

Never Again
```

---

## With Dependencies

```text
Mount

↓

Run Effect

↓

Dependency Changes

↓

Cleanup

↓

Run Again
```

---

## No Dependency Array

```text
Render

↓

Effect

↓

Render

↓

Effect

↓

Render

↓

Effect
```

Every render.

---

# Why Cleanup?

Without cleanup

```
Timer

↓

Still Running

↓

Memory Leak
```

With cleanup

```
Timer

↓

clearInterval()

↓

Removed
```

---

# Common Mistakes

## Infinite Loop

Wrong

```jsx
useEffect(()=>{

setCount(

count+1

);

});
```

No dependency array.

Every render updates state.

Infinite loop.

---

Correct

```jsx
useEffect(()=>{

setCount(

prev=>prev+1

);

},[]);
```

or redesign the logic if repeated updates are intended.

---

## Missing Dependency

Wrong

```jsx
useEffect(()=>{

console.log(

count

);

},[]);
```

The effect won't react to future `count` changes.

---

Correct

```jsx
useEffect(()=>{

console.log(

count

);

},[count]);
```

---

## Forgetting Cleanup

Wrong

```jsx
useEffect(()=>{

window.addEventListener(

"resize",

resize

);

},[]);
```

The listener remains even after unmount.

---

Correct

```jsx
useEffect(()=>{

window.addEventListener(

"resize",

resize

);

return()=>{

window.removeEventListener(

"resize",

resize

);

};

},[]);
```

---

# useEffect vs useLayoutEffect

| useEffect | useLayoutEffect |
|------------|-----------------|
| Runs after browser paint | Runs before browser paint |
| Doesn't block UI | Can block painting |
| API calls | DOM measurements |
| Most common | Special cases |

---

# Best Practices

- Keep one responsibility per effect.
- Always include the correct dependencies.
- Clean up subscriptions, timers, and listeners.
- Don't use `useEffect` for calculations that can be done during rendering.
- Avoid unnecessary effects.

---

# Interview Questions

## Beginner

### What is useEffect?

A Hook used to perform side effects after rendering.

---

### What is a side effect?

Any operation outside rendering, such as fetching data, timers, subscriptions, or interacting with browser APIs.

---

### Why do we use dependency arrays?

To control when an effect runs.

---

# Intermediate

### Difference between

```jsx
[]
```

and

```jsx
[count]
```

| [] | [count] |
|----|----------|
| Runs once | Runs whenever count changes |

---

### Why do we need cleanup?

To prevent memory leaks and remove resources that are no longer needed.

---

### Can we have multiple useEffect hooks?

Yes.

Using multiple focused effects is encouraged because each effect should handle a single responsibility.

---

# Senior-Level Interview Questions

### When exactly does useEffect run?

After React commits updates to the DOM and the browser has painted the UI.

---

### Why shouldn't you fetch data directly inside the component function?

The component function executes on every render.

Fetching inside it would repeatedly trigger network requests.

`useEffect` lets you control when those requests happen.

---

### Explain the lifecycle using useEffect.

```text
Render

↓

Commit

↓

useEffect

↓

State Update

↓

Cleanup

↓

New Effect

↓

Unmount

↓

Final Cleanup
```

---

### What causes an infinite loop in useEffect?

Updating state inside an effect that runs after every render (or whose dependency list causes it to re-run indefinitely).

---

# Scenario-Based Questions

## Scenario 1

A timer continues running after leaving the page.

Why?

**Answer**

The interval wasn't cleared in the cleanup function.

---

## Scenario 2

An API is called every second even though it should run only once.

**Answer**

The dependency array is missing.

Use:

```jsx
useEffect(()=>{

fetchData();

},[]);
```

---

## Scenario 3

Your application has multiple unrelated side effects.

Should you combine them?

**Answer**

No.

Split them into multiple `useEffect` hooks based on responsibility.

---

## Scenario 4

An interviewer asks:

**"Does useEffect replace componentDidMount?"**

**Answer**

Not exactly.

With an empty dependency array (`[]`), it behaves similarly to `componentDidMount`, but `useEffect` is a more general mechanism for synchronizing a component with external systems and can also handle updates and cleanup.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Hook | `useEffect()` |
| Purpose | Side Effects |
| Runs | After render and commit |
| `[]` | Once after mount |
| `[deps]` | On dependency change |
| No array | After every render |
| Cleanup | Return a function |
| Common Uses | API, Timer, Events, WebSocket, Local Storage |

---

# Excalidraw Diagram

```text
                    Component Render

                           │

                    Virtual DOM

                           │

                    Commit Phase

                           │

                   Browser Paint

                           │

                     useEffect()

                           │

        ┌──────────────────┼──────────────────┐

        │                  │                  │

     API Call          Event Listener      Timer

        │                  │                  │

        └──────────────────┼──────────────────┘

                           │

                    State Update

                           │

                      Re-render

                           │

                        Cleanup

                           │

                     Run Effect Again

                           │

                      Component Unmount

                           │

                      Final Cleanup
```

---

# Related Notes

- 