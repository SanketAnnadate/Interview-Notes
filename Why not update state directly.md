---
title: Why Should You Not Update State Directly in React?
tags:
  - react
  - state
  - usestate
  - interview
  - react-internals
---

# Why Should You Not Update State Directly in React?

> [!INFO]
> **State should never be updated directly.**
>
> Always use:
>
> - `setState()` (Class Components)
> - `setState` function returned by `useState()` (Functional Components)

Updating state directly **does not inform React that the state has changed**, so React cannot schedule a re-render.

---

# Beginner Explanation

Imagine your mobile banking app displays:

```
Available Balance

₹50,000
```

If the actual balance changes to ₹49,500 but the screen still shows ₹50,000,

the data and UI are no longer synchronized.

React State works the same way.

Changing the variable directly changes the data **only in memory**.

React doesn't know anything changed.

Therefore,

**the UI never updates.**

---

# Why Does React Need setState()?

React is responsible for rendering the UI.

Whenever state changes,

React must know:

- What changed?
- Which component changed?
- Which DOM elements should update?
- Should rendering be delayed?
- Can updates be batched?

React gets all this information only when you use:

```jsx
setState()
```

or

```jsx
setCount()
```

---

# Incorrect Way

## Functional Component

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    function increment() {

        count++;

    }

    return (
        <>
            <h1>{count}</h1>

            <button onClick={increment}>
                Increment
            </button>
        </>
    );
}
```

---

## What Happens?

```
count becomes 1

↓

React doesn't know

↓

No Re-render

↓

UI still shows

0
```

---

# Correct Way

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    function increment() {

        setCount(count + 1);

    }

    return (
        <>
            <h1>{count}</h1>

            <button onClick={increment}>
                Increment
            </button>
        </>
    );
}
```

---

## What Happens?

```
setCount()

↓

React receives update

↓

Render scheduled

↓

Virtual DOM updated

↓

Diffing

↓

Commit

↓

Real DOM updated

↓

UI becomes

1
```

---

# Class Component Example

### ❌ Wrong

```jsx
this.state.count = this.state.count + 1;
```

No re-render occurs.

---

### ✅ Correct

```jsx
this.setState({

    count: this.state.count + 1

});
```

React updates the component correctly.

---

# Internal Working

When you write

```jsx
setCount(10);
```

React performs the following steps.

```
setCount()

        │

        ▼

Create Update Object

        │

        ▼

Store Update in Fiber Queue

        │

        ▼

Scheduler Assigns Priority

        │

        ▼

Component Re-renders

        │

        ▼

New Virtual DOM

        │

        ▼

Diffing Algorithm

        │

        ▼

Commit Phase

        │

        ▼

Only Changed DOM Updated
```

---

# What Happens During Direct Mutation?

```
count++

        │

        ▼

JavaScript Variable Changes

        │

        ▼

React NOT Notified

        │

        ▼

No Render

        │

        ▼

Old UI Remains
```

---

# Why React Doesn't Detect Direct Mutation

React compares **references**, not individual object properties.

Suppose

```jsx
const [user, setUser] = useState({

    name: "John",

    age: 30

});
```

Wrong

```jsx
user.age = 31;
```

Reference

```
Before

User
↓

Memory A
```

```
After Mutation

User
↓

Memory A
```

The reference is the same.

React assumes nothing changed.

---

Correct

```jsx
setUser({

    ...user,

    age:31

});
```

Now

```
Before

Memory A
```

↓

```
After

Memory B
```

React detects a new reference and updates the UI.

---

# Array Example

Wrong

```jsx
const [skills, setSkills] = useState([
    "React"
]);

skills.push("Java");
```

No re-render.

---

Correct

```jsx
setSkills([

    ...skills,

    "Java"

]);
```

React receives a new array reference.

---

# Real World Example

## Banking Application

Balance

```
₹50,000
```

Wrong

```jsx
balance = 49500;
```

UI

```
₹50,000
```

Still incorrect.

---

Correct

```jsx
setBalance(49500);
```

UI

```
₹49,500
```

---

# Another Example

Shopping Cart

Before

```
Items

Laptop

Total

₹60,000
```

Wrong

```jsx
cart.push(mouse);
```

React doesn't update.

User still sees

```
₹60,000
```

---

Correct

```jsx
setCart([

...cart,

mouse

]);
```

React recalculates the total and updates the UI.

---

# Why Immutability Matters

React expects state to be **immutable**.

Instead of changing existing data,

create new data.

Example

Instead of

```
Old Object

↓

Modify It
```

Do

```
Old Object

↓

Create New Object

↓

Update UI
```

Benefits

- Faster comparison
- Predictable rendering
- Easier debugging
- Undo/Redo support
- Better performance

---

# Common Mistakes

## Direct Increment

❌

```jsx
count++;
```

---

## Direct Assignment

❌

```jsx
count = 5;
```

---

## Object Mutation

❌

```jsx
user.name = "Alex";
```

---

## Array Mutation

❌

```jsx
items.push(item);
```

---

# Best Practices

✅ Always use `setState()` or `setCount()`.

✅ Never modify state directly.

✅ Use the spread operator for objects and arrays.

✅ Treat state as immutable.

✅ Use functional updates when the next state depends on the previous state.

```jsx
setCount(prev => prev + 1);
```

---

# Interview Questions

## Basic

### Why shouldn't we update state directly?

Because React won't know that the state has changed, so it won't schedule a re-render and the UI can become inconsistent.

---

### What happens if we write?

```jsx
count++;
```

The JavaScript variable changes, but React does not re-render the component.

---

### How should state be updated?

Using:

```jsx
setCount(newValue);
```

or

```jsx
this.setState(...)
```

---

# Senior-Level Interview Questions

### Why does React rely on immutable updates?

React efficiently detects changes by comparing object references. Creating new objects or arrays allows React to determine what changed without deeply comparing every property.

---

### What happens internally when `setState()` is called?

1. React creates an Update object.
2. It places the update into the Fiber update queue.
3. The Scheduler assigns a priority.
4. React re-renders the component.
5. A new Virtual DOM is created.
6. Reconciliation compares the previous and new trees.
7. The Commit Phase updates only the necessary DOM nodes.

---

### Why doesn't React detect direct mutation?

Because the object's reference remains the same. React assumes nothing has changed and skips updating the UI.

---

# Scenario-Based Questions

## Scenario 1

A developer writes:

```jsx
user.age++;

console.log(user.age);
```

The console shows the updated age, but the UI still displays the old value.

Why?

**Answer**

The object was mutated directly. React was never notified of the change because the state setter was not used.

---

## Scenario 2

A shopping cart total doesn't update after adding a new product.

The code is:

```jsx
cart.push(product);
```

What's wrong?

**Answer**

`push()` mutates the existing array. React doesn't detect a new reference.

Correct approach:

```jsx
setCart([
    ...cart,
    product
]);
```

---

## Scenario 3

An interview panel asks:

**"Why is React's immutability model important?"**

**Expected Answer**

Immutability enables efficient change detection, predictable rendering, easier debugging, better compatibility with memoization, and improved performance through reference comparison.

---

# Quick Revision

| Wrong | Correct |
|--------|---------|
| `count++` | `setCount(count + 1)` |
| `user.name = "Alex"` | `setUser({ ...user, name: "Alex" })` |
| `array.push(item)` | `setArray([...array, item])` |
| Direct Mutation | Immutable Update |
| React Doesn't Re-render | React Re-renders |

---

# Related Notes

- 