---
title: How can useRef be used to store mutable values?
tags:
  - react
  - hooks
  - useref
  - mutable
  - interview
created: 2026-08-03
---

# How can useRef be used to store mutable values?

> [!INFO]
> `useRef` can store **any mutable value** that persists across component re-renders **without causing a re-render** when the value changes.

> [!IMPORTANT]
> Updating `ref.current` **does NOT trigger a component re-render**.
>
> React only re-renders when **state** or **props** change.

---

# Beginner Explanation

Think of `useRef` as a **private storage box** inside your component.

```
Component

↓

Storage Box (useRef)

↓

current = value
```

You can

- Read it
- Update it
- Store anything

React remembers it between renders.

Unlike **useState**, changing it **doesn't refresh the screen**.

---

# Syntax

```jsx
const myRef = useRef(initialValue);
```

Example

```jsx
const counterRef = useRef(0);
```

Internally

```jsx
{
   current: 0
}
```

---

# Updating Mutable Values

```jsx
import { useRef } from "react";

function Counter() {

    const counterRef = useRef(0);

    function increment() {
        counterRef.current++;
        console.log(counterRef.current);
    }

    return (
        <button onClick={increment}>
            Increment
        </button>
    );
}
```

Console Output

```
1
2
3
4
5
```

Notice

The button never re-renders.

Only the console changes.

---

# Visual Flow

```text
Button Click

↓

counterRef.current++

↓

Value Updated

↓

React ignores it

↓

No Re-render
```

---

# Why Doesn't React Re-render?

React watches

- State
- Props

React **does not watch**

```jsx
ref.current
```

So

```jsx
counterRef.current++;
```

updates the value,

but React doesn't know the UI should refresh.

---

# Mutable vs Immutable

### Mutable Value

Can change without creating a new object.

```jsx
counterRef.current = 5;
counterRef.current = 10;
counterRef.current = 20;
```

Same object.

Only the value changes.

---

### State

```jsx
setCount(count + 1);
```

Creates a new state value.

React detects the change.

React re-renders.

---

# useRef vs useState

| Feature | useRef | useState |
|----------|---------|----------|
| Stores Value | ✅ | ✅ |
| Causes Re-render | ❌ | ✅ |
| UI Updates Automatically | ❌ | ✅ |
| Mutable | ✅ | ❌ (update through setter) |
| Best For | Internal mutable values | UI state |

---

# Real World Use Cases

## 1. Store Previous Value

```jsx
const previousValue = useRef();

useEffect(() => {
    previousValue.current = value;
}, [value]);
```

Useful for comparing

```
Old Value

↓

New Value
```

---

## 2. Render Counter

```jsx
const renderCount = useRef(0);

renderCount.current++;

console.log(renderCount.current);
```

Great for debugging.

---

## 3. Store Timer

```jsx
const timerRef = useRef();

timerRef.current = setInterval(() => {
    console.log("Running");
}, 1000);

clearInterval(timerRef.current);
```

---

## 4. Store WebSocket Connection

```jsx
const socketRef = useRef();

socketRef.current = new WebSocket(url);
```

The socket survives re-renders.

---

## 5. Store API AbortController

```jsx
const controllerRef = useRef();

controllerRef.current = new AbortController();
```

---

## 6. Store Previous Scroll Position

```jsx
const scrollRef = useRef(0);

scrollRef.current = window.scrollY;
```

---

## 7. Cache Expensive Objects

```jsx
const cacheRef = useRef({});
```

---

## 8. Store Form Dirty State

```jsx
const dirtyRef = useRef(false);

dirtyRef.current = true;
```

---

# Internal Working

```text
Component Render

↓

useRef()

↓

{

current:0

}

↓

Button Click

↓

current++

↓

Value Changed

↓

Same Ref Object

↓

React Doesn't Re-render
```

---

# Common Mistakes

## Mistake 1

Using ref for UI

```jsx
countRef.current++;

return <h1>{countRef.current}</h1>;
```

The UI won't update.

Use `useState` instead.

---

## Mistake 2

Using state for timers

```jsx
const [timer, setTimer] = useState();
```

This causes unnecessary re-renders.

Prefer

```jsx
const timerRef = useRef();
```

---

## Mistake 3

Expecting React to detect ref changes

```jsx
ref.current = "Hello";
```

React won't update the screen automatically.

---

# When Should You Use Mutable Values?

Use `useRef` when the value:

- Doesn't affect the UI.
- Must survive re-renders.
- Needs to be updated frequently.
- Represents an external object (timer, socket, DOM node).

Examples:

- Timer IDs
- Interval IDs
- Previous values
- WebSocket instances
- API controllers
- Scroll positions
- DOM elements
- Cache objects

---

# When NOT to Use useRef

Don't use `useRef` for:

- Counter displayed on screen
- Theme
- Logged-in user shown in UI
- Form values displayed to the user

These belong in **state**.

---

# Senior-Level Interview Questions

### Why does updating `ref.current` not trigger a re-render?

React compares **state** and **props** to decide when to render. The `current` property of a ref is mutable and isn't part of React's rendering mechanism, so changing it doesn't notify React.

---

### Why is useRef considered mutable?

Because the same ref object is preserved across renders, and you can directly modify its `current` property without creating a new object.

---

### Is useRef recreated on every render?

No.

React creates the ref object only once and returns the same object on every render.

---

### Can useRef store objects and arrays?

Yes.

```jsx
const userRef = useRef({
    id: 1,
    name: "John"
});

const listRef = useRef([]);
```

It can store any JavaScript value.

---

# Scenario-Based Interview Questions

## Scenario 1

You need to count button clicks for analytics but don't want the UI to update.

**Answer**

Use

```jsx
const clickCountRef = useRef(0);
```

Increment

```jsx
clickCountRef.current++;
```

---

## Scenario 2

You need to store a WebSocket connection.

**Answer**

```jsx
const socketRef = useRef();
```

Store the connection in

```jsx
socketRef.current
```

---

## Scenario 3

You need to remember the previous search query.

**Answer**

```jsx
const previousSearch = useRef();
```

Update it inside `useEffect`.

---

## Scenario 4

You need to save an interval ID.

**Answer**

```jsx
const intervalRef = useRef();

intervalRef.current = setInterval(...);

clearInterval(intervalRef.current);
```

---

## Scenario 5 (Senior)

The interviewer asks:

**Why would you choose useRef instead of state?**

**Answer**

Because the value doesn't affect rendering. Using state would trigger unnecessary re-renders and reduce performance.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Stores Mutable Values | ✅ |
| Persists Across Renders | ✅ |
| Causes Re-render | ❌ |
| Access Value | `ref.current` |
| Mutable | ✅ |
| UI Updates | ❌ |
| Best For | Timers, Previous Values, Cache, DOM |

---

# Excalidraw Diagram

```text
                    useRef()

                       │

               current Property

                       │

        ┌──────────────┼──────────────┐
        │              │              │

   Timer ID      Previous Value    Counter

        │              │              │

 Update current   Update current  Update current

        │              │              │

        └──────────────┼──────────────┘

                       │

              Mutable Value Changed

                       │

               Same Object Reference

                       │

             React Doesn't Detect Change

                       │

                No Component Re-render


────────────────────────────────────────────

                useState

setState()

      │

      ▼

State Changed

      │

      ▼

React Re-render

      │

      ▼

UI Updated


────────────────────────────────────────────

                useRef

ref.current++

      │

      ▼

Value Changed

      │

      ▼

Same Object

      │

      ▼

No Re-render
```

---

# Related Notes

- [[Hooks]]
- [[useRef]]