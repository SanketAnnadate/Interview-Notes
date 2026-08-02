---
title: What is setState Callback in React?
tags:
  - react
  - state
  - setstate
  - callback
  - class-component
  - interview
---

# What is setState Callback in React?

> [!INFO]
> The **setState callback** is a function passed as the **second argument** to `setState()` in **Class Components**. It executes **after the state has been updated and the component has finished re-rendering**.

> [!WARNING]
> The `setState` callback is **only available in Class Components**.
>
> Functional Components using `useState` **do not support** a callback as the second argument.

---

# Syntax

```jsx
this.setState(
    {
        count: this.state.count + 1
    },
    () => {
        console.log("State Updated");
    }
);
```

The callback runs **only after**:

1. State has been updated.
2. Component has re-rendered.
3. Updated UI is visible.

---

# Why Do We Need a Callback?

React updates state **asynchronously**.

Consider this example:

```jsx
this.setState({
    count: this.state.count + 1
});

console.log(this.state.count);
```

### Expected Output

```
1
```

### Actual Output

```
0
```

Why?

Because `setState()` schedules the update instead of updating the state immediately.

---

# Using setState Callback

```jsx
this.setState(
    {
        count: this.state.count + 1
    },
    () => {
        console.log(this.state.count);
    }
);
```

Output

```
1
```

The callback executes only after React finishes updating the component.

---

# Complete Example

```jsx
import React, { Component } from "react";

class Counter extends Component {

    state = {
        count: 0
    };

    increment = () => {

        this.setState(
            {
                count: this.state.count + 1
            },
            () => {
                console.log(
                    "Updated Count:",
                    this.state.count
                );
            }
        );
    };

    render() {

        return (
            <div>

                <h2>{this.state.count}</h2>

                <button onClick={this.increment}>
                    Increment
                </button>

            </div>
        );
    }
}

export default Counter;
```

---

# Execution Flow

```text
Button Click

        │

        ▼

setState()

        │

        ▼

React Schedules Update

        │

        ▼

State Updated

        │

        ▼

Component Re-rendered

        │

        ▼

Callback Function Executes
```

---

# Internal Working

When `setState()` is called:

```text
setState()

↓

Update Object Created

↓

Added to Update Queue

↓

Scheduler

↓

Render Phase

↓

Virtual DOM

↓

Reconciliation

↓

Commit Phase

↓

State Updated

↓

Callback Executes
```

The callback always runs **after the Commit Phase**.

---

# Real-World Example

Suppose a banking application updates the account balance.

```jsx
this.setState(
    {
        balance: 50000
    },
    () => {

        sendNotification();

    }
);
```

The notification is sent **only after** the balance is updated on the screen.

---

# Functional Components

The following is **NOT supported**:

```jsx
setCount(
    count + 1,
    () => {
        console.log("Done");
    }
);
```

This will not work.

---

# How to Achieve the Same Behavior in Functional Components?

Use `useEffect`.

```jsx
const [count, setCount] = useState(0);

useEffect(() => {

    console.log("Updated");

}, [count]);
```

Whenever `count` changes,

`useEffect` runs after the component has rendered.

---

# Class Component vs Functional Component

| Class Component | Functional Component |
|----------------|----------------------|
| `setState()` callback | `useEffect()` |
| Callback supported | Callback not supported |
| Legacy approach | Modern React |

---

# Common Mistakes

## ❌ Reading State Immediately

```jsx
this.setState({
    count: 1
});

console.log(this.state.count);
```

May print the old value.

---

## ✅ Correct

```jsx
this.setState(
    {
        count: 1
    },
    () => {

        console.log(this.state.count);

    }
);
```

---

## ❌ Using Callback with useState

```jsx
setCount(
    count + 1,
    () => {}
);
```

Invalid.

Use `useEffect`.

---

# Best Practices

- Use the callback only in Class Components.
- Prefer `useEffect` in Functional Components.
- Avoid depending on immediate state updates.
- Keep callback logic small and focused.

---

# Interview Questions

### What is the `setState` callback?

It is a function passed as the second argument to `setState()` that runs after the state update is completed and the component has re-rendered.

---

### Why is the callback needed?

Because `setState()` is asynchronous and you may need to perform an action after the updated state is reflected in the UI.

---

### Does `useState` support callbacks?

No.

Functional Components use `useEffect()` to perform actions after state changes.

---

### What is the modern replacement for the `setState` callback?

`useEffect()`.

Example:

```jsx
useEffect(() => {

    console.log("Count Updated");

}, [count]);
```

---

# Senior-Level Interview Questions

### Why was the `setState` callback removed in Hooks?

Hooks separate state updates (`useState`) from side effects (`useEffect`), making component logic clearer and more predictable. Instead of passing callbacks into state setters, React encourages handling post-render side effects in `useEffect`.

---

### When exactly does the callback execute?

After:

1. State update is processed.
2. Render phase completes.
3. Commit phase updates the DOM.

Only then is the callback executed.

---

### Is the callback executed before rendering?

No.

It always executes **after** React has committed the updated UI.

---

# Scenario-Based Questions

## Scenario 1

A developer writes:

```jsx
this.setState({
    count: 10
});

console.log(this.state.count);
```

The console prints the old value.

Why?

**Answer**

`setState()` schedules the update asynchronously. The log runs before the update is committed.

---

## Scenario 2

You need to call an API only after the component displays the latest state.

How would you do it?

**Class Component**

```jsx
this.setState(
    {
        data: result
    },
    () => {

        callAPI();

    }
);
```

**Functional Component**

```jsx
useEffect(() => {

    callAPI();

}, [data]);
```

---

## Scenario 3

A team migrates from Class Components to Functional Components.

How do you replace `setState` callbacks?

**Answer**

Move the callback logic into a `useEffect()` that depends on the updated state.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| `setState` Callback | Second argument of `setState()` |
| Available In | Class Components |
| Executes | After state update and re-render |
| `useState` Callback | ❌ Not Supported |
| Modern Alternative | `useEffect()` |

---

# Related Notes

- 