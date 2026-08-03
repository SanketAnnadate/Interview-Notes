---
title: useRef Hook
tags:
  - react
  - hooks
  - useref
  - interview
  - performance
---

# useRef Hook

> [!INFO]
> `useRef` is a React Hook that allows you to **store mutable values** and **access DOM elements** without causing a component to re-render.

---

# Definition

`useRef` returns a mutable object whose value is stored inside its `current` property.

```jsx
const ref = useRef(initialValue);
```

Example

```jsx
const inputRef = useRef(null);
```

Internally

```jsx
{
    current: null
}
```

---

# Why use useRef?

React components re-render whenever **state** or **props** change.

Sometimes we need to store data that

- Should survive re-renders
- Doesn't affect the UI
- Should not trigger another render

This is where **useRef** is useful.

---

# Primary Uses

## 1. Access DOM Elements

Most common use case.

```jsx
const inputRef = useRef(null);
```

```jsx
<input ref={inputRef} />
```

Access DOM

```jsx
inputRef.current.focus();
```

---

## 2. Store Mutable Values

```jsx
const countRef = useRef(0);

countRef.current++;
```

Unlike state,

this **does not** re-render the component.

---

# Syntax

```jsx
const myRef = useRef(initialValue);
```

Example

```jsx
const timerRef = useRef(null);
```

---

# Understanding `.current`

Everything inside a ref is stored in

```jsx
ref.current
```

Example

```jsx
const countRef = useRef(0);

console.log(countRef.current);
```

Output

```
0
```

Update

```jsx
countRef.current = 10;
```

Output

```
10
```

---

# How useRef Works

```text
Component Render

↓

useRef()

↓

{

current: value

}

↓

Update current

↓

Same Object

↓

No Re-render
```

---

# Example 1 – Focus Input

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef(null);

    function focusInput() {
        inputRef.current.focus();
    }

    return (
        <>
            <input ref={inputRef} />
            <button onClick={focusInput}>
                Focus
            </button>
        </>
    );
}
```

Output

```
Click Button

↓

Input gets focused
```

---

# Example 2 – Store Counter

```jsx
import { useRef } from "react";

function Counter() {

    const countRef = useRef(0);

    function increment() {
        countRef.current++;
        console.log(countRef.current);
    }

    return (
        <button onClick={increment}>
            Increment
        </button>
    );
}
```

Console

```
1
2
3
4
5
```

No re-render occurs.

---

# Example 3 – Previous Value

```jsx
const previousValue = useRef();

useEffect(() => {
    previousValue.current = value;
}, [value]);
```

Useful for comparing

```
Previous Value

↓

Current Value
```

---

# Example 4 – Store Timer

```jsx
const timerRef = useRef();

useEffect(() => {

    timerRef.current = setInterval(() => {
        console.log("Running");
    }, 1000);

    return () => {
        clearInterval(timerRef.current);
    };

}, []);
```

---

# Example 5 – Scroll to Bottom

```jsx
const messageRef = useRef(null);

messageRef.current.scrollIntoView();
```

---

# Example 6 – Play Video

```jsx
const videoRef = useRef();

videoRef.current.play();

videoRef.current.pause();
```

---

# Example 7 – File Upload

```jsx
const fileInputRef = useRef();

fileInputRef.current.click();
```

---

# Example 8 – Store API Controller

```jsx
const controllerRef = useRef();

controllerRef.current = new AbortController();
```

---

# Example 9 – Cache Expensive Object

```jsx
const cacheRef = useRef({});
```

---

# useRef vs useState

| Feature | useRef | useState |
|----------|---------|-----------|
| Causes Re-render | ❌ No | ✅ Yes |
| Stores Value | ✅ Yes | ✅ Yes |
| Mutable | ✅ Yes | ❌ No |
| UI Updates | ❌ No | ✅ Yes |
| Best For | DOM, Timers, Previous Values | UI State |

---

# useRef vs createRef

| useRef | createRef |
|---------|-----------|
| Functional Components | Mostly Class Components |
| Same object every render | New object every render |
| Better Performance | Less Efficient in Functional Components |

---

# Common Use Cases

- Focus an input
- Scroll to an element
- Store timers
- Store interval IDs
- Store previous values
- Store WebSocket connections
- Store AbortController
- Store mutable counters
- Integrate third-party libraries
- Store DOM references

---

# Common Mistakes

## ❌ Expecting UI to Update

```jsx
countRef.current++;
```

UI will **not** update.

Use state if the value is displayed.

---

## ❌ Using useRef Instead of State

Wrong

```jsx
const username = useRef("John");
```

If username appears in UI,

use

```jsx
useState()
```

---

## ❌ Accessing Ref Before Mount

Wrong

```jsx
inputRef.current.focus();
```

before the input exists.

---

# Best Practices

- Use refs for DOM manipulation.
- Use refs for mutable values that don't affect rendering.
- Never replace state with refs for UI data.
- Keep imperative DOM operations minimal.

---

# Real-World Scenarios

## Chat Application

```
New Message

↓

Scroll to Bottom

↓

scrollIntoView()
```

---

## Login Page

```
Page Loads

↓

Focus Username Input
```

---

## Stopwatch

```
Timer ID

↓

Store in useRef

↓

Stop Timer

↓

clearInterval()
```

---

## Video Player

```
Play Button

↓

videoRef.current.play()
```

---

## Canvas

```
Canvas Reference

↓

Get Context

↓

Draw Shapes
```

---

# Interview Questions

## Beginner

### What is useRef?

A hook used to access DOM elements and store mutable values without causing re-renders.

---

### Does updating `ref.current` re-render the component?

No.

---

### What property stores the value?

```jsx
ref.current
```

---

## Intermediate

### Difference between useRef and useState?

- `useState` triggers re-renders.
- `useRef` does not.

---

### Can useRef store objects?

Yes.

It can store

- Objects
- Arrays
- Functions
- Timers
- DOM Elements
- Any JavaScript value

---

## Senior-Level Questions

### Why doesn't updating `ref.current` trigger a re-render?

Because React only tracks changes to **state** and **props**. Mutating the `current` property doesn't notify React.

---

### Why use useRef for timers?

Timer IDs don't affect the UI. Storing them in state would cause unnecessary re-renders.

---

### When should you avoid useRef?

Avoid using it for values that determine what is rendered on the screen. Use `useState` or `useReducer` instead.

---

# Scenario-Based Questions

## Scenario 1

You want to focus an input after the page loads.

**Answer**

Use

```jsx
useRef + useEffect
```

---

## Scenario 2

You need to store an interval ID.

**Answer**

```jsx
const timerRef = useRef();
```

---

## Scenario 3

You need to keep the previous search value.

**Answer**

```jsx
const previousSearch = useRef();
```

Update it inside `useEffect`.

---

## Scenario 4

You want to store a WebSocket connection.

**Answer**

```jsx
const socketRef = useRef();

socketRef.current = new WebSocket(url);
```

---

## Scenario 5

The interviewer asks:

**Why not use state instead of useRef?**

**Answer**

Using state causes re-renders. If the value doesn't affect the UI (timers, DOM references, previous values, sockets), `useRef` is more efficient.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Hook | useRef |
| Returns | `{ current }` |
| Causes Re-render | ❌ No |
| Stores Mutable Values | ✅ Yes |
| Stores DOM Reference | ✅ Yes |
| UI Updates | ❌ No |
| Best Use Cases | DOM, Timers, Previous Values, External Objects |

---

# Excalidraw Mind Map

```text
                     useRef()

                          │

        ┌─────────────────┴─────────────────┐
        │                                   │

   DOM Reference                     Mutable Values

        │                                   │

   Focus Input                      Previous Value

   Scroll                           Timer ID

   Video                            Interval ID

   Canvas                           Cache

   File Upload                      WebSocket

        │                                   │

        └─────────────────┬─────────────────┘

                          │

                  No Component Re-render

                          │

                  Better Performance


──────────────────────────────────────────

            useState vs useRef

useState                      useRef

State                         current

   │                              │

Update                      Update

   │                              │

Re-render                 No Re-render

   │                              │

UI Updated              Internal Value
```

---

# Related Notes

- [[Hooks]]
- 
```