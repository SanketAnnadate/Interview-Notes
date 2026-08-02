---
title: Difference Between State and Props in React
tags:
  - react
  - state
  - props
  - interview
  - react-basics
created: 2026-08-02
---

# Difference Between State and Props in React

> [!INFO]
> **State** and **Props** are two fundamental concepts in React.
>
> They are often confused, but they serve completely different purposes.
>
> Understanding the difference is one of the most common React interview questions.

---

# Beginner Explanation

Imagine a family.

👨 Father

👦 Son

The father gives the son a toy.

The son **cannot change** the toy.

The father can replace it.

The toy is like **Props**.

---

Now imagine the son has a piggy bank.

He can save money.

He can spend money.

He controls it himself.

The piggy bank is like **State**.

---

## Simple Analogy

```text
Parent
      │
      │ gives data
      ▼
Child

Props = Data given by Parent

State = Data owned by Child
```

---

# What is State?

State is data that belongs to a component.

The component itself controls the data.

The component can update it whenever required.

Example

```jsx
const [count, setCount] = useState(0);
```

Only this component controls `count`.

---

# What are Props?

Props are data passed from a parent component to a child component.

The child can read them,

but cannot modify them.

Example

```jsx
<User name="Sanket" />
```

Child

```jsx
function User({ name }) {

    return <h1>{name}</h1>;

}
```

---

# Visual Representation

```text
                Parent Component

               State = "John"

                     │

                     │ Props

                     ▼

              Child Component

          Receives "John"

          Cannot Modify It
```

---

# Example Using State

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    return (

        <button
            onClick={() => setCount(count + 1)}
        >
            {count}
        </button>

    );

}
```

State changes

```
0

↓

1

↓

2

↓

3
```

---

# Example Using Props

Parent

```jsx
function App() {

    return (

        <User

            name="Sanket"

        />

    );

}
```

Child

```jsx
function User({

    name

}) {

    return (

        <h1>

            {name}

        </h1>

    );

}
```

Output

```
Sanket
```

---

# Can Child Modify Props?

❌ No

Wrong

```jsx
props.name = "Rahul";
```

Never modify props.

---

# Can Component Modify State?

✅ Yes

```jsx
setCount(count + 1);
```

---

# State vs Props

| Feature | State | Props |
|----------|--------|--------|
| Meaning | Internal data of a component | Data received from parent |
| Ownership | Component itself | Parent component |
| Mutable | ✅ Yes (using setter function) | ❌ No (Read-only) |
| Updated By | Component itself | Parent component |
| Purpose | Store changing data | Pass data to child components |
| Re-render | When state changes | When parent passes new props |
| Scope | Local to component | Shared with child |
| Controlled By | Current Component | Parent Component |
| Can Child Modify? | Not applicable | ❌ No |
| Hook | useState() | No hook required |

---

# State and Props Together

```text
App

State

↓

Props

↓

User Component

↓

Display UI
```

Example

```jsx
function App() {

    const [name, setName] = useState("Sanket");

    return (

        <User

            name={name}

        />

    );

}
```

Child

```jsx
function User({

    name

}) {

    return (

        <h2>

            {name}

        </h2>

    );

}
```

When

```jsx
setName("Rahul");
```

React updates

```
Sanket

↓

Rahul
```

The child receives the updated prop automatically.

---

# Real-World Example

## Banking Application

Parent

```text
Bank Dashboard
```

State

```text
Balance = ₹50,000
```

Passes

```jsx
<AccountCard

    balance={50000}

/>
```

Child

Displays

```
₹50,000
```

When Parent updates

```jsx
setBalance(49000);
```

Child automatically receives

```
₹49,000
```

---

# Internal Working

## State Update

```text
setState()

↓

React Scheduler

↓

Component Re-render

↓

Virtual DOM

↓

Diffing

↓

Real DOM
```

---

## Props Update

```text
Parent State Changes

↓

Parent Re-renders

↓

New Props Generated

↓

Child Receives Props

↓

Child Re-renders

↓

Virtual DOM

↓

Real DOM
```

---

# When to Use State?

Use State when:

- Counter
- Form Data
- Loading Indicator
- Selected Item
- API Response
- Theme Toggle
- Modal Open/Close

---

# When to Use Props?

Use Props when:

- Passing username
- Passing product details
- Passing event handlers
- Passing configuration
- Reusable components
- Parent-to-child communication

---

# Common Mistakes

## Using Props Instead of State

Wrong

```jsx
<User

count={0}

/>
```

Trying to change

```jsx
props.count++;
```

Not allowed.

---

## Mutating Props

Wrong

```jsx
props.name = "Alex";
```

Props are immutable.

---

## Duplicating Props into State

```jsx
const [name] = useState(props.name);
```

Usually unnecessary unless you intentionally need a separate editable copy.

---

# Best Practices

✅ Keep State as small as possible.

✅ Props should remain immutable.

✅ Lift State up when multiple components need the same data.

✅ Pass only required Props.

✅ Don't duplicate Props into State unless necessary.

---

# Interview Questions

## Beginner

### What is State?

State is internal data managed by a component.

---

### What are Props?

Props are read-only values passed from a parent component to a child component.

---

### Which one is mutable?

State.

---

### Which one is immutable?

Props.

---

### Who owns State?

The component itself.

---

### Who owns Props?

The parent component.

---

# Intermediate

### Can Props change?

Yes.

Only when the parent passes new values.

---

### Does changing State update Props?

Indirectly.

When the parent's state changes and it passes the new value as a prop, child components receive updated props.

---

### Why are Props read-only?

To maintain predictable one-way data flow and prevent child components from accidentally modifying parent data.

---

# Senior-Level Interview Questions

### Explain the complete flow from Parent State to Child Props.

**Expected Answer**

```text
Parent State Changes

↓

Parent Re-renders

↓

Creates New Props

↓

Child Receives New Props

↓

Child Re-renders

↓

Virtual DOM Updated

↓

Diffing

↓

Commit Phase

↓

Real DOM Updated
```

---

### Why did React choose one-way data flow?

One-way data flow makes applications predictable, easier to debug, easier to test, and reduces unintended side effects between components.

---

### When would you lift state up?

When multiple sibling components need access to the same data. Move the state to their closest common parent and pass it down through props.

---

### Can Props contain functions?

Yes.

Functions are commonly passed as props so child components can notify the parent about user actions.

---

# Scenario-Based Questions

## Scenario 1

A child component needs to update the parent's counter.

How can it do this?

**Answer**

The parent passes a callback function as a prop.

Child calls the callback.

Parent updates its own state.

---

## Scenario 2

A developer writes

```jsx
props.user.name = "Alex";
```

Why is this wrong?

**Answer**

Props are immutable. Child components must never modify data owned by the parent.

---

## Scenario 3

Two sibling components need the same data.

Where should the state be stored?

**Answer**

Lift the state to the nearest common parent and pass it down as props.

---

## Scenario 4

When would you use Context API instead of Props?

**Answer**

When data must be shared across many nested components and passing props through multiple intermediate levels (prop drilling) becomes difficult to maintain.

---

# Quick Revision

| State | Props |
|--------|--------|
| Internal Data | External Data |
| Mutable | Immutable |
| Managed by Component | Managed by Parent |
| Updated using `setState()` / `useState()` | Updated by Parent |
| Local | Passed to Child |
| Used for Dynamic Data | Used for Communication |

---

# Related Notes

- 