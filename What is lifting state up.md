---
title: What is Lifting State Up in React?
tags:
  - react
  - state
  - props
  - lifting-state-up
  - interview
  - react-architecture
created: 2026-08-02
---

# What is Lifting State Up in React?

> [!INFO]
> **Lifting State Up** is a React design pattern where **state is moved from child components to their closest common parent component** so that multiple child components can share and synchronize the same data.

> [!IMPORTANT]
> React follows the principle:
>
> **Single Source of Truth**
>
> Shared state should exist in **one place only**, usually the nearest common parent.

---

# Beginner Explanation

Imagine two siblings.

```
Brother

Sister
```

Both want to know the family's WiFi password.

### Option 1

Each child stores the password.

```
Brother

Password = abc123

Sister

Password = abc123
```

Problem

If the password changes,

both children must update it separately.

Data becomes inconsistent.

---

### Option 2 (Best)

Parent stores the password.

```
Father

Password = xyz789

↓

↓

Brother      Sister
```

Whenever the password changes,

both children automatically receive the updated password.

This is exactly what **Lifting State Up** means.

---

# Why Do We Need Lifting State Up?

Suppose we have two counters.

```
Counter A

Counter B
```

Both should always display the same count.

If each component has its own state,

```
Counter A = 5

Counter B = 2
```

They become inconsistent.

Instead,

move the state to the parent.

```
App

Count = 5

↓

↓

Counter A

Counter B
```

Now both always display the same value.

---

# Before Lifting State Up

```text
App

│

├── Counter A

│      count = 0

│

└── Counter B

       count = 0
```

Each component manages its own state.

No synchronization.

---

# After Lifting State Up

```text
App

count = 0

│

├───────────────┐

▼               ▼

Counter A    Counter B
```

State exists only once.

Children receive it through props.

---

# Example Without Lifting State

## CounterA.jsx

```jsx
import { useState } from "react";

function CounterA() {

    const [count, setCount] = useState(0);

    return (

        <button
            onClick={() => setCount(count + 1)}
        >
            Counter A : {count}
        </button>

    );

}

export default CounterA;
```

---

## CounterB.jsx

```jsx
import { useState } from "react";

function CounterB() {

    const [count, setCount] = useState(0);

    return (

        <button
            onClick={() => setCount(count + 1)}
        >
            Counter B : {count}
        </button>

    );

}

export default CounterB;
```

Output

```
Counter A = 5

Counter B = 2
```

Both components have different states.

---

# Example With Lifting State Up

## App.jsx

```jsx
import { useState } from "react";
import Counter from "./Counter";

function App() {

    const [count, setCount] = useState(0);

    function handleIncrement() {
        setCount(prev => prev + 1);
    }

    return (

        <>

            <Counter

                title="Counter A"

                count={count}

                onIncrement={handleIncrement}

            />

            <Counter

                title="Counter B"

                count={count}

                onIncrement={handleIncrement}

            />

        </>

    );

}

export default App;
```

---

## Counter.jsx

```jsx
function Counter({

    title,

    count,

    onIncrement

}) {

    return (

        <div>

            <h2>{title}</h2>

            <h3>{count}</h3>

            <button
                onClick={onIncrement}
            >
                Increment
            </button>

        </div>

    );

}

export default Counter;
```

---

Output

```
Counter A = 5

Counter B = 5
```

Clicking either button updates **both** counters because they share the same state.

---

# How Lifting State Up Works

```text
User Clicks Button

        │

        ▼

Child Component

        │

Calls Parent Callback

        │

        ▼

Parent Updates State

        │

        ▼

Parent Re-renders

        │

        ▼

Updated Props Sent

        │

        ▼

All Children Re-render
```

---

# Internal Working

```text
Child

↓

onClick()

↓

Parent Callback

↓

setState()

↓

React Scheduler

↓

Render Phase

↓

Virtual DOM

↓

Diffing

↓

Commit Phase

↓

Updated Props

↓

Children Render Again
```

---

# Single Source of Truth

One state

Multiple components

```
App

count

↓

↓

↓

Counter

Chart

Summary
```

All UI remains synchronized.

---

# Real-World Example

## Shopping Cart

```
App

↓

Cart State

↓

↓

↓

Navbar

Product List

Checkout
```

All components display the same cart.

Updating one place updates every component automatically.

---

## Banking Application

```
Dashboard

↓

Balance State

↓

↓

↓

Header

Account Card

Transaction History
```

When balance changes,

all three components update.

---

# Advantages

- Single Source of Truth
- Easier Debugging
- No Duplicate State
- Consistent UI
- Better Maintainability
- Easier Testing
- Predictable Data Flow

---

# Disadvantages

- More props may need to be passed.
- Can lead to **Prop Drilling** if the component tree is very deep.
- For deeply nested applications, Context API or state management libraries may be a better choice.

---

# Lifting State Up vs Prop Drilling

| Lifting State Up | Prop Drilling |
|------------------|---------------|
| Good Design Pattern | Problem caused by deep trees |
| Move state to parent | Pass props through many unnecessary components |
| Recommended | Avoid when excessive |
| Solves shared state | Can make code harder to maintain |

---

# When Should You Lift State Up?

Use Lifting State Up when:

- Multiple components need the same data.
- Sibling components must stay synchronized.
- Data belongs to the parent.
- A single source of truth is required.

---

# When Should You Avoid It?

Avoid excessive lifting when:

- State is only used in one component.
- The parent becomes overloaded with unrelated state.
- Prop drilling becomes excessive.

Consider:

- Context API
- Redux Toolkit
- Zustand
- Jotai

---

# Common Mistakes

## Duplicate State

```jsx
Counter A

count = 5

Counter B

count = 5
```

Two copies of the same information.

Avoid this.

---

## Keeping Shared State in Child

Wrong

```
Child owns data

↓

Sibling needs data
```

Instead,

move it to the common parent.

---

## Prop Drilling

```
App

↓

Dashboard

↓

Sidebar

↓

Menu

↓

Button
```

Passing the same props through many intermediate components can become difficult to maintain.

---

# Best Practices

- Keep a single source of truth.
- Lift state only to the nearest common ancestor.
- Don't lift state unnecessarily.
- Pass callback functions to children for updates.
- Keep components reusable.
- Use Context API for deeply shared state.

---

# Interview Questions

## Beginner

### What is Lifting State Up?

Moving state from child components to their closest common parent so multiple components can share the same data.

---

### Why do we use Lifting State Up?

To share state between sibling components while keeping a single source of truth.

---

### What is Single Source of Truth?

Having only one authoritative copy of shared data.

---

# Intermediate

### How does a child update lifted state?

The parent passes a callback function as a prop.

The child calls that callback.

The parent updates its own state.

---

### Does lifting state improve consistency?

Yes.

All components read from the same state.

---

# Senior-Level Interview Questions

### How do you decide where state should live?

**Expected Answer**

State should live in the **lowest common ancestor** of all components that need access to it.

Keep state as close as possible to where it is used while avoiding duplication.

---

### What problems can excessive lifting cause?

- Prop drilling
- Large parent components
- Unnecessary re-renders
- Difficult maintenance

Possible solutions:

- Context API
- Redux Toolkit
- Zustand
- Component composition

---

### Does lifting state affect performance?

Potentially.

When the parent state changes, all children receiving updated props may re-render.

Optimization techniques include:

- `React.memo`
- `useMemo`
- `useCallback`
- Splitting components
- Keeping state localized when possible

---

# Scenario-Based Questions

## Scenario 1

Two sibling components must always display the same selected user.

How would you design it?

**Answer**

Store the selected user state in their closest common parent and pass it to both children via props.

---

## Scenario 2

A Product List updates the shopping cart, and the Navbar should immediately show the new item count.

Where should the cart state live?

**Answer**

In a common parent (or a global store if many parts of the application need it), then pass the cart data to both components.

---

## Scenario 3

After lifting state, your parent component has become very large.

What would you do?

**Answer**

- Split the parent into smaller feature components.
- Move unrelated state closer to where it's used.
- Use Context API or a state management library if appropriate.
- Memoize components where beneficial.

---

## Scenario 4

An interviewer asks:

**"When should you use Context API instead of Lifting State Up?"**

**Answer**

Use Lifting State Up for a small number of related components.

Use Context API when many deeply nested components need the same data and prop drilling becomes excessive.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Lifting State Up | Move shared state to the nearest common parent |
| Purpose | Share state between components |
| Benefit | Single Source of Truth |
| Child Updates Parent | Callback functions passed as props |
| Problem Solved | Duplicate state |
| Possible Issue | Prop Drilling |
| Alternatives | Context API, Redux Toolkit, Zustand |

---

# Related Notes

- [[State]]
- [[Props]]
- [[Props vs State]]
- [[How to Update State in React]]
- [[Prop Drilling]]
- [[Context API]]
- [[useState]]
- [[React Component Architecture]]
```