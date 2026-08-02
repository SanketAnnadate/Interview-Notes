---
title: Conditional Rendering in React
tags:
  - react
  - conditional-rendering
  - jsx
  - interview
  - rendering
created: 2026-08-02
---

# Conditional Rendering in React

> [!INFO]
> **Conditional Rendering** means displaying different UI based on a condition.

Just like JavaScript uses **if**, **else**, **&&**, and the **ternary operator (`? :`)**, React uses the same concepts to decide **what should be rendered**.

---

# Beginner Explanation

Imagine a shopping mall.

```
Customer Logged In ?

        │

   Yes       No

    │         │

Dashboard   Login Page
```

The UI changes based on a condition.

React works exactly the same way.

---

# Why Do We Need Conditional Rendering?

Real applications constantly change the UI depending on:

- Login Status
- User Role
- Loading State
- API Response
- Permissions
- Theme
- Screen Size
- Feature Flags

Example

```
Loading...

↓

Data Loaded

↓

Show Products
```

---

# Ways to Conditionally Render

```
Conditional Rendering

        │

        ├── if / else

        ├── Ternary Operator

        ├── && Operator

        ├── Return null

        ├── Switch Statement

        ├── Variables

        └── Component Mapping
```

---

# 1. Using if / else

```jsx
function Greeting({

    isLoggedIn

}) {

    if (isLoggedIn) {

        return <h1>Welcome Back</h1>;

    }

    return <h1>Please Login</h1>;

}
```

Output

```
true

↓

Welcome Back
```

```
false

↓

Please Login
```

---

# 2. Using Ternary Operator (Most Common)

```jsx
function App({

    isLoggedIn

}) {

    return (

        <>

            {

                isLoggedIn

                    ?

                    <Dashboard />

                    :

                    <Login />

            }

        </>

    );

}
```

---

# 3. Using && Operator

Render only if condition is true.

```jsx
function App({

    isAdmin

}) {

    return (

        <>

            {

                isAdmin &&

                <AdminPanel />

            }

        </>

    );

}
```

If

```
isAdmin = false
```

Nothing renders.

---

# 4. Returning null

```jsx
function User({

    visible

}) {

    if (!visible)

        return null;

    return <h2>User Profile</h2>;

}
```

Nothing is rendered.

Useful for hiding components.

---

# 5. Using Variables

```jsx
function App({

    isLoggedIn

}) {

    let content;

    if (isLoggedIn)

        content = <Dashboard />;

    else

        content = <Login />;

    return content;

}
```

Useful for complex logic.

---

# 6. Switch Statement

```jsx
function Status({

    role

}) {

    switch(role){

        case "admin":

            return <Admin />;

        case "manager":

            return <Manager />;

        default:

            return <User />;

    }

}
```

---

# 7. Component Mapping (Advanced)

Instead of

```jsx
if...

else...

```

Use

```jsx
const screens = {

    home: <Home />,

    login: <Login />,

    dashboard: <Dashboard />

};

return screens[page];
```

Very common in enterprise applications.

---

# Real World Example

## Login

```text
User Logged In ?

        │

   Yes       No

    │         │

Dashboard   Login Page
```

---

## Loading

```jsx
return loading

?

<Spinner/>

:

<ProductList/>
```

---

## User Role

```
Role

↓

Admin

↓

Admin Dashboard
```

```
Role

↓

Customer

↓

Customer Dashboard
```

---

## Feature Flag

```
Premium User

↓

Premium Features

↓

Else

↓

Basic Features
```

---

# Internal Working

```
Component Render

↓

Evaluate Condition

↓

Choose JSX

↓

Virtual DOM

↓

Diffing

↓

Commit

↓

Updated UI
```

---

# Conditional Rendering vs CSS

Wrong

```jsx
<div

style={{

display:"none"

}}

>

Dashboard

</div>
```

Component is still mounted.

---

Better

```jsx
{

show &&

<Dashboard/>

}
```

Component isn't rendered at all.

---

# Nested Ternary (Avoid)

Wrong

```jsx
condition1

?

condition2

?

<A/>

:

<B/>

:

<C/>
```

Hard to read.

Better

Use

```
if

else

```

or separate components.

---

# Best Practices

✅ Use **if/else** for complex logic.

✅ Use **ternary** for simple two-way decisions.

✅ Use **&&** when rendering something only if true.

✅ Return **null** to hide components.

✅ Avoid deeply nested ternary operators.

---

# Performance Considerations

React only renders the branch that evaluates to true.

Example

```jsx
{

isAdmin

&&

<HeavyComponent/>

}
```

If

```
isAdmin = false
```

React never renders `HeavyComponent`.

---

# Common Mistakes

## Using if inside JSX

Wrong

```jsx
return(

<div>

if(isLoggedIn)

<h1>Hello</h1>

</div>

)
```

Not valid.

---

Correct

```jsx
{

isLoggedIn

?

<h1>Hello</h1>

:

null

}
```

---

## Using && with Numbers

```jsx
{

0 &&

<Component/>

}
```

Output

```
0
```

React renders the number `0`.

Better

```jsx
{

count > 0

&&

<Component/>

}
```

---

# Interview Questions

## Beginner

### What is Conditional Rendering?

Rendering different UI based on a condition.

---

### Which JavaScript operators are commonly used?

- if / else
- Ternary (`? :`)
- &&
- Switch

---

### Which operator is most commonly used?

Ternary Operator.

---

### Which operator renders only when the condition is true?

```
&&
```

---

# Intermediate

### Difference between `&&` and Ternary?

| && | Ternary |
|----|----------|
| Renders only when true | Chooses between two outputs |
| No else block | Has else block |

---

### Why return `null`?

To prevent rendering while keeping the component mounted in the React tree.

---

### Which is better?

```
if

else
```

or

```
Ternary
```

Use

- **if/else** → Complex logic
- **Ternary** → Simple UI decisions

---

# Senior-Level Interview Questions

### Does conditional rendering affect performance?

Yes.

React only renders the branch that evaluates to true.

However,

frequent mounting/unmounting of expensive components may have a cost.

Optimization techniques include

- React.memo
- Lazy Loading
- Suspense

---

### What happens when a condition changes?

```
State Changes

↓

Component Re-renders

↓

Condition Re-evaluated

↓

New Virtual DOM

↓

Diffing

↓

Commit Phase

↓

Updated UI
```

---

### Difference between hiding with CSS and conditional rendering?

| CSS (`display:none`) | Conditional Rendering |
|----------------------|-----------------------|
| Component stays mounted | Component isn't rendered |
| Effects still exist | Effects cleaned up on unmount |
| DOM node exists | DOM node removed |

---

# Scenario-Based Questions

## Scenario 1

Your application shows a spinner while fetching data.

How would you implement it?

```jsx
return loading

?

<Spinner/>

:

<ProductList/>
```

---

## Scenario 2

A button should only be visible to administrators.

```jsx
{

isAdmin

&&

<DeleteButton/>

}
```

---

## Scenario 3

An interviewer asks:

**"Should you use nested ternary operators?"**

**Answer**

Avoid deeply nested ternaries because they reduce readability.

Prefer

- if/else
- separate helper functions
- smaller components

---

## Scenario 4

A premium feature should only be visible when

```
isPremium === true
```

How would you implement it?

```jsx
{

isPremium

&&

<PremiumFeatures/>

}
```

---

# Quick Revision

| Technique | Best Use Case |
|-----------|---------------|
| if / else | Complex conditions |
| Ternary | Two-way rendering |
| && | Render only when true |
| return null | Hide component |
| switch | Multiple cases |
| Mapping Object | Dynamic component rendering |

---

# Excalidraw Diagram

```text
                    Component

                         │

                  Evaluate Condition

                         │

        ┌────────────────┼─────────────────┐

        │                │                 │

      if/else         Ternary            &&

        │                │                 │

      JSX A          JSX A / JSX B      JSX

        │                │                 │

        └────────────────┼─────────────────┘

                         │

                    Virtual DOM

                         │

                    Reconciliation

                         │

                     Updated UI
```

---

# Related Notes

- 
```