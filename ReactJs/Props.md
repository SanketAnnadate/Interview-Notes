---
title: What are Props in React?
tags:
  - react
  - props
  - components
  - interview
  - react-basics
created: 2026-08-02
---

# What are Props in React?

> [!INFO]
> **Props (Properties)** are read-only data passed from a **Parent Component** to a **Child Component**. They allow components to communicate while maintaining React's **one-way data flow**.

> [!IMPORTANT]
> **Props are immutable.**
>
> A child component **cannot modify** the props it receives.
>
> Only the parent component can update props.

---

# Beginner Explanation

Imagine a father gives his son a toy.

```
Father

↓

Toy

↓

Son
```

The son can play with the toy,

but **cannot change what the father gave him**.

Similarly,

```
Parent Component

↓

Props

↓

Child Component
```

The child can **use** the props,

but **cannot modify** them.

---

# Why Do We Need Props?

React applications consist of many reusable components.

Instead of hardcoding values,

we pass data using props.

Without Props

```jsx
function Greeting() {
    return <h1>Hello John</h1>;
}
```

Always prints

```
Hello John
```

---

With Props

```jsx
function Greeting(props) {

    return <h1>Hello {props.name}</h1>;

}
```

Now

```jsx
<Greeting name="John" />

<Greeting name="David" />

<Greeting name="Alex" />
```

Output

```
Hello John

Hello David

Hello Alex
```

One component,

multiple outputs.

---

# How Props Work

```
Parent Component

↓

Creates Props

↓

Child Component Receives Props

↓

Displays Data
```

---

# Basic Example

## Child Component

```jsx
function Greeting(props) {

    return (

        <h1>

            Hello {props.name}

        </h1>

    );

}

export default Greeting;
```

---

## Parent Component

```jsx
import Greeting from "./Greeting";

function App() {

    return (

        <>

            <Greeting name="John" />

            <Greeting name="David" />

            <Greeting name="Alex" />

        </>

    );

}

export default App;
```

---

Output

```
Hello John

Hello David

Hello Alex
```

---

# Props are Read Only

Suppose

```jsx
function Greeting(props) {

    props.name = "Rahul";

    return <h1>{props.name}</h1>;

}
```

❌ Never do this.

Props should never be modified.

Instead,

Parent should update them.

---

# Destructuring Props

Instead of

```jsx
function Greeting(props) {

    return <h1>{props.name}</h1>;

}
```

Use

```jsx
function Greeting({ name }) {

    return <h1>{name}</h1>;

}
```

Cleaner,

more readable,

used in almost every React project.

---

# Passing Multiple Props

```jsx
<User

    name="Sanket"

    age={31}

    city="Pune"

/>
```

Receiving

```jsx
function User({

    name,

    age,

    city

}) {

    return (

        <>

            <h2>{name}</h2>

            <p>{age}</p>

            <p>{city}</p>

        </>

    );

}
```

---

# Passing Objects

```jsx
const employee = {

    name:"John",

    salary:80000

};

<Employee employee={employee} />
```

Receiving

```jsx
function Employee({

    employee

}) {

    return (

        <>

            {employee.name}

        </>

    );

}
```

---

# Passing Arrays

```jsx
const skills = [

    "React",

    "Java",

    "Spring"

];

<Skills skills={skills}/>
```

---

# Passing Functions

Functions can also be passed as props.

Parent

```jsx
function App() {

    function welcome() {

        alert("Welcome");

    }

    return (

        <Button

            onClick={welcome}

        />

    );

}
```

Child

```jsx
function Button({

    onClick

}) {

    return (

        <button onClick={onClick}>

            Click

        </button>

    );

}
```

This is how child components communicate back to the parent.

---

# Children Prop

React automatically provides a special prop called **children**.

Parent

```jsx
<Card>

    <h2>Hello</h2>

    <p>Welcome</p>

</Card>
```

Child

```jsx
function Card({

    children

}) {

    return (

        <div>

            {children}

        </div>

    );

}
```

Output

```
+--------------------+

Hello

Welcome

+--------------------+
```

---

# Default Props

Modern React commonly uses default values during destructuring.

```jsx
function User({

    name = "Guest"

}) {

    return <h2>{name}</h2>;

}
```

If no prop is passed,

output becomes

```
Guest
```

---

# One-Way Data Flow

React follows

```
Parent

↓

Child

↓

Grand Child
```

Data always flows downward.

Children cannot directly modify parent props.

---

# Props vs State

| Props | State |
|--------|--------|
| Passed from Parent | Managed inside Component |
| Read Only | Mutable through setter |
| External Data | Internal Data |
| Parent Controls | Component Controls |
| Immutable | Updated using `setState()` / `useState()` |

---

# Real World Example

## Banking Application

```
App

↓

Account

↓

BalanceCard
```

App receives

```json
{
    "balance":50000
}
```

Passes

```jsx
<BalanceCard

    balance={50000}

/>
```

Child

```jsx
function BalanceCard({

    balance

}) {

    return (

        <h2>

            ₹{balance}

        </h2>

    );

}
```

---

# Internal Working

```
Parent Render

↓

Creates Props Object

↓

Child Receives Props

↓

Child Renders JSX

↓

Virtual DOM

↓

Diffing

↓

Real DOM
```

Whenever parent props change,

React re-renders the child with the updated values.

---

# Common Mistakes

## ❌ Modifying Props

```jsx
props.name = "Alex";
```

Never modify props.

---

## ❌ Using Props Instead of State

Props

```
Read Only
```

State

```
Can Change
```

---

## ❌ Prop Drilling

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

Passing props through many levels is called **Prop Drilling**.

Solutions

- Context API
- Redux Toolkit
- Zustand

---

# Best Practices

✅ Keep props immutable.

✅ Destructure props.

✅ Pass only required props.

✅ Avoid deeply nested prop drilling.

✅ Use meaningful prop names.

✅ Use TypeScript or PropTypes for validation in larger projects.

---

# Interview Questions

## Beginner

### What are Props?

Props are read-only values passed from a parent component to a child component.

---

### Why are Props used?

To pass data, configuration, and event handlers between components.

---

### Can a child modify Props?

No.

Props are immutable.

---

### What is One-Way Data Flow?

Data always flows

```
Parent

↓

Child
```

Children cannot directly modify parent data.

---

# Intermediate

### Difference between Props and State?

| Props | State |
|--------|--------|
| Read Only | Mutable |
| Parent Controls | Component Controls |
| Passed Down | Created Inside Component |

---

### Can functions be passed as Props?

Yes.

Functions are commonly passed so child components can notify parents about events.

---

### What is `children`?

A special prop used to render nested JSX inside a component.

---

# Senior-Level Interview Questions

### Why are Props immutable?

Immutability makes components predictable and easier to debug. It preserves React's one-way data flow and allows React to optimize rendering.

---

### When does a child component re-render because of Props?

A child re-renders when the parent passes new prop values or new object/function references. Memoization techniques such as `React.memo` can prevent unnecessary renders when props haven't changed.

---

### What is Prop Drilling?

Passing the same props through multiple intermediate components that don't use them.

Possible solutions:

- Context API
- Redux Toolkit
- Zustand

---

### How does React compare Props?

For normal components, React re-renders when the parent renders.

For memoized components (`React.memo`), React performs a **shallow comparison** of props to decide whether a re-render is necessary.

---

# Scenario-Based Questions

## Scenario 1

A child component changes

```jsx
props.name = "Rahul";
```

Why is this wrong?

**Answer**

Props are immutable. Only the parent should update the value.

---

## Scenario 2

Your application passes the same prop through six components.

What is this problem called?

**Answer**

Prop Drilling.

Solutions:

- Context API
- Redux Toolkit
- Zustand

---

## Scenario 3

A memoized child component still re-renders every time.

Why?

**Answer**

The parent is creating a new object or function on every render.

Example:

```jsx
<User

    data={{name:"John"}}

/>
```

A new object reference is created on each render.

Possible solutions:

- `useMemo`
- `useCallback`
- Move object creation outside the render if appropriate

---

## Scenario 4

When should you use Props and when should you use State?

**Answer**

Use **Props** for data owned by a parent and passed to children.

Use **State** for data that belongs to and changes within the component.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Props | Read-only data passed from Parent to Child |
| Mutable | ❌ No |
| One-Way Data Flow | Parent → Child |
| Functions as Props | ✅ Yes |
| Objects as Props | ✅ Yes |
| Arrays as Props | ✅ Yes |
| `children` | Special prop for nested JSX |
| Prop Drilling | Passing props through many levels |

---

# Related Notes

- [[States vs Props]]
- [[Props]]
    ↔ [[Children Prop]]
    ↔ [[Default props]]
    ↔ [[Prop Drilling]]
    ↔ [[Context API]]
    ↔ [[Props vs State]]