# What is State in React?

> [!INFO] Definition
> **State** is a built-in React mechanism used to store data that can change over time. When the state changes, React automatically re-renders the component to reflect the updated data in the UI.

---

# Beginner Explanation

Imagine a whiteboard in your room.

Initially, the whiteboard contains:

```

Count = 0

```

When someone writes:

```

Count = 1

```

the whiteboard changes.

React State works exactly like this.

It stores changing information.

Whenever the information changes, React updates the UI automatically.

---

# Real World Example

Instagram Likes

Initially

❤️ 25 Likes

User clicks Like

❤️ 26 Likes

What changed?

Only the **state**.

React automatically updates the screen.

---

# Another Example

Bank Account Dashboard

Before Transaction

```

Balance = ₹50,000

```

After sending ₹500

```

Balance = ₹49,500

```

The **balance is state**.

The UI updates automatically.

---

# Why Do We Need State?

Without State

```jsx
let count = 0;

function increment() {
    count++;
}
```

The variable changes,

but the UI **does not**.

---

With State

```jsx
const [count, setCount] = useState(0);

function increment() {
    setCount(count + 1);
}
```

React automatically updates the UI.

---

# How State Works

```

User clicks button

↓

setState()

↓

React schedules update

↓

Virtual DOM created

↓

Diffing

↓

Reconciliation

↓

Real DOM updated

↓

Browser paints screen

```

---

# Anatomy of useState()

```jsx
const [count, setCount] = useState(0);
```

| Part | Meaning |
|------|---------|
| count | Current State |
| setCount | Updates State |
| useState | React Hook |
| 0 | Initial Value |

---

# Example

```jsx
import { useState } from "react";

function Counter() {

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

---

# What Happens Internally?

Many developers know **how** to use `useState`, but interviewers at top companies often ask **how it works internally**.

Internally, React stores state values in the Fiber tree.

A simplified view:

```

Fiber Node

↓

Hook List

↓

State Queue

↓

Current Value

```

When you call:

```jsx
setCount(5)
```

React does **not** update the UI immediately.

Instead, it:

1. Creates an update object.
2. Places it in the update queue.
3. Schedules a render.
4. Recalculates the component.
5. Compares the new Virtual DOM with the old one.
6. Updates only the changed DOM nodes.

---

# Is State Mutable?

No.

State should always be treated as **immutable**.

❌ Wrong

```jsx
user.name = "John";
```

✅ Correct

```jsx
setUser({
    ...user,
    name: "John"
});
```

Why?

React detects changes using **new object references**.

---

# Types of State

## Local State

```jsx
const [count, setCount] = useState(0);
```

Only this component can access it.

---

## Shared State

Multiple components need it.

Example

Shopping Cart

```

Navbar

↓

Cart Icon

↓

Checkout

↓

Payment

```

Possible solutions:

- Context API
- Redux Toolkit
- Zustand

---

# When Should You Use State?

Use State when:

✅ UI changes

✅ User input

✅ API response

✅ Loading indicator

✅ Selected item

✅ Form values

---

Do NOT use State for

❌ Constants

❌ Static values

❌ Configuration

---

# State vs Variable

| Variable | State |
|-----------|------|
| UI doesn't update | UI updates |
| Plain JavaScript | Managed by React |
| Lost on re-render | Preserved by React |

---

# State vs Props

| State | Props |
|--------|-------|
| Mutable | Immutable |
| Owned by Component | Passed by Parent |
| Updated with setState | Read Only |

---

# Real Production Example

Netflix

```

Movies

↓

Loading...

↓

Movies Loaded

↓

Render Movies

```

Loading is State.

Movies List is State.

Error Message is State.

---

# Common Mistakes

## 1. Direct Mutation

❌

```jsx
user.name = "Alex";
```

---

## 2. Multiple State Variables

Instead of

```jsx
const [firstName,setFirstName]
const [lastName,setLastName]
const [email,setEmail]
const [phone,setPhone]
```

Sometimes better

```jsx
const [user,setUser]
```

---

## 3. Storing Derived State

❌

```jsx
const [total,setTotal]
```

When it can be calculated.

Use

```jsx
const total = price * quantity;
```

---

# Performance Considerations

Every state update causes a re-render.

This does **not** necessarily mean the entire page is updated.

React updates only the affected components after reconciliation.

---

# Best Practices

✅ Keep state as small as possible.

✅ Lift state only when necessary.

✅ Don't duplicate state.

✅ Never mutate state.

✅ Prefer functional updates when the next value depends on the previous one.

```jsx
setCount(prev => prev + 1);
```

---

# Senior-Level Interview Questions

## What happens when setState() is called?

Expected Answer

1. React creates an Update object.
2. Pushes it into the Hook Update Queue.
3. Scheduler decides priority.
4. Fiber tree begins rendering.
5. Virtual DOM is recreated.
6. Diffing Algorithm compares trees.
7. Commit Phase updates the Real DOM.

---

## Is setState synchronous?

No.

React batches updates and schedules rendering.

With automatic batching (React 18+), multiple updates inside the same event are combined into a single render for better performance.

---

## Why shouldn't state be mutated?

React relies on object identity (references) to detect changes efficiently. Mutating existing objects can prevent React from recognizing updates and lead to stale UI.

---

## Why does React re-render after state changes?

Because state represents dynamic data used to render the UI. A state update causes React to re-run the component function, produce a new Virtual DOM, compare it with the previous one, and apply only the necessary DOM updates.

---

# Scenario-Based Questions

## Scenario 1

A component re-renders every second.

How would you investigate?

Expected Answer

- Check which state changes every second.
- Use React DevTools Profiler.
- Identify unnecessary state updates.
- Memoize expensive child components if appropriate.

---

## Scenario 2

You update an object inside state.

The UI does not update.

Why?

Answer

The object reference did not change because it was mutated directly.

---

## Scenario 3

The application becomes slow after adding Context.

What would you do?

Answer

- Split Context into smaller contexts.
- Memoize context values.
- Move frequently changing state closer to where it is used.
- Consider Redux Toolkit or Zustand for complex global state.

---

## Scenario 4

A component has 40 state variables.

How would you redesign it?

Answer

- Group related state into objects.
- Use `useReducer` for complex state transitions.
- Extract logic into custom Hooks where appropriate.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| State | Stores dynamic data |
| Hook | useState |
| Update Function | setState |
| Mutable? | ❌ No |
| Causes Re-render? | ✅ Yes |
| Lives In | React Fiber |
| Detects Changes Using | New Object References |
| Shared State | Context / Redux / Zustand |
| Best Practice | Keep state minimal and immutable |

---

# Related Notes

- [[Update State]]
- [[useCallBack]]
- [[Why not update state directly]]
- [[States vs Props]]
- [[What is lifting state up]]
- [[State]]
    ↔ [[useState]]
    ↔ [[setState]]
    ↔ [[Batching]]
    ↔ [[Lifting State Up]]
    ↔ [[Immutable Updates]]
    ↔ [[Props vs State]]