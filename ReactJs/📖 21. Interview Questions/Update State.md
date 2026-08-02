---
title: How to Update State in React
tags:
  - react
  - state
  - usestate
  - hooks
  - interview
  - react-internals
  - senior
created: 2026-08-01
---
---
title: How to Update State in React
tags:
  - react
  - state
  - usestate
  - hooks
  - interview
---

# How to Update State in React?

> [!INFO]
> State in React is updated using:
>
> - **useState Hook** in Functional Components (Modern React)
> - **setState() Method** in Class Components (Legacy React)

---

# Why Do We Update State?

State stores data that changes over time.

When the state changes, React automatically **re-renders** the component and updates the UI.

Example:

Before Clicking Button

```text
Count = 0
```

After Clicking Button

```text
Count = 1
```

---

# Ways to Update State

## 1. Functional Components (Recommended)

State is updated using the setter function returned by `useState`.

### Syntax

```jsx
const [state, setState] = useState(initialValue);
```

### Example

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    function handleClick() {
        setCount(count + 1);
    }

    return (
        <div>
            <p>You clicked {count} times</p>

            <button onClick={handleClick}>
                Click Me
            </button>
        </div>
    );
}

export default Counter;
```

---

## Output

Initially

```text
You clicked 0 times
```

After Clicking Once

```text
You clicked 1 times
```

After Clicking Again

```text
You clicked 2 times
```

---

## 2. Class Components (Legacy)

State is updated using `this.setState()`.

### Example

```jsx
import React, { Component } from "react";

class Counter extends Component {

    state = {
        count: 0
    };

    increment = () => {
        this.setState({
            count: this.state.count + 1
        });
    };

    render() {
        return (
            <div>

                <p>You clicked {this.state.count} times</p>

                <button onClick={this.increment}>
                    Click Me
                </button>

            </div>
        );
    }
}

export default Counter;
```

> [!NOTE]
> Class Components are still supported but are rarely used in new React projects.

---

# How React Updates State Internally

```text
User Clicks Button
        │
        ▼
setCount()
        │
        ▼
React Schedules Update
        │
        ▼
Component Re-renders
        │
        ▼
Virtual DOM Updated
        │
        ▼
Diffing
        │
        ▼
Only Changed DOM Updated
```

---

# Updating State Based on Previous State

When the new state depends on the previous state, use a **functional update**.

### ❌ Incorrect

```jsx
setCount(count + 1);
setCount(count + 1);
```

Result

```text
Count increases by only 1
```

---

### ✅ Correct

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Result

```text
Count increases by 2
```

---

# Updating Object State

### ❌ Wrong

```jsx
user.name = "John";
```

### ✅ Correct

```jsx
setUser({
    ...user,
    name: "John"
});
```

---

# Updating Array State

### ❌ Wrong

```jsx
skills.push("React");
```

### ✅ Correct

```jsx
setSkills([
    ...skills,
    "React"
]);
```

---

# Best Practices

- Always use the state setter function.
- Never modify state directly.
- Use functional updates when the next value depends on the previous value.
- Keep state immutable.
- Prefer functional components with Hooks.

---

# Common Mistakes

### Direct Mutation

```jsx
count++;
```

### Mutating Objects

```jsx
user.age = 30;
```

### Mutating Arrays

```jsx
list.push(item);
```

These changes do **not** notify React to re-render.

---

# Interview Questions

### How do you update state in React?

Using the `useState` setter function in functional components or `setState()` in class components.

---

### Why shouldn't state be updated directly?

Because React tracks state changes through its updater functions. Direct mutation bypasses React's rendering mechanism.

---

### Which approach is recommended today?

Functional Components with the `useState` Hook.

---

### What happens when state is updated?

1. React schedules the update.
2. The component re-renders.
3. React compares the new Virtual DOM with the previous one.
4. Only the changed DOM elements are updated.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Functional Component | `setState` function from `useState` |
| Class Component | `this.setState()` |
| Direct Mutation | ❌ Never |
| Object Update | Spread Operator (`...`) |
| Array Update | Spread Operator (`...`) |
| Causes Re-render | ✅ Yes |

---

# Related Notes

- 