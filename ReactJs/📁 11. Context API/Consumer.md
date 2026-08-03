---
title: Context API Consumer
tags:
  - react
  - context-api
  - consumer
  - usecontext
  - interview
created: 2026-08-02
---
# Context API Consumer

> [!INFO]
> A **Consumer** is any React component that **reads data from a Context Provider**.

> [!IMPORTANT]
> In modern React, we use the **`useContext()` Hook** instead of the older `<Context.Consumer>` component.

---

# Beginner Explanation

Imagine a classroom.

```
Teacher

↓

Announcement

↓

Students Listen
```

The teacher is the **Provider**.

The students are the **Consumers**.

Similarly,

```
Context Provider

↓

Shared Data

↓

Consumer Component
```

The Consumer receives the shared data.

---

# Context API Architecture

```text
createContext()

↓

Provider

↓

Shared Data

↓

Consumer

↓

Render UI
```

---

# Who is the Consumer?

Any component that uses

```jsx
useContext()
```

or

```jsx
<Context.Consumer>
```

is called a **Consumer**.

---

# Modern Consumer (Recommended)

## Step 1

Create Context

```jsx
import {

createContext

} from "react";

export const UserContext=

createContext();
```

---

## Step 2

Provider

```jsx
<UserContext.Provider

value={{

name:"John"

}}

>

<Home/>

</UserContext.Provider>
```

---

## Step 3

Consumer

```jsx
import {

useContext

} from "react";

import {

UserContext

} from "./UserContext";

function Profile(){

const user=

useContext(

UserContext

);

return(

<h2>

{user.name}

</h2>

);

}
```

---

# Visual Flow

```text
UserContext.Provider

↓

Stores Value

↓

React Context

↓

Profile Component

↓

useContext(UserContext)

↓

Receives Value

↓

Display Data
```

---

# Complete Example

## UserContext.js

```jsx
import {

createContext

} from "react";

export const UserContext=

createContext();
```

---

## App.jsx

```jsx
import {

UserContext

} from "./UserContext";

import Profile from "./Profile";

function App(){

const user={

name:"John",

city:"London"

};

return(

<UserContext.Provider

value={user}

>

<Profile/>

</UserContext.Provider>

);

}
```

---

## Profile.jsx

```jsx
import {

useContext

} from "react";

import {

UserContext

} from "./UserContext";

function Profile(){

const user=

useContext(

UserContext

);

return(

<>

<h2>

{user.name}

</h2>

<p>

{user.city}

</p>

</>

);

}
```

---

# Old Consumer Syntax (Legacy)

Before React Hooks

```jsx
<UserContext.Consumer>

{

(user)=>(

<h1>

{user.name}

</h1>

)

}

</UserContext.Consumer>
```

This still works but is rarely used in new projects.

---

# Modern vs Legacy

| useContext | Context.Consumer |
|------------|------------------|
| Modern | Legacy |
| Cleaner | More verbose |
| Function Components | Classes & Functions |
| Recommended | Mostly old codebases |

---

# Consuming Multiple Contexts

```jsx
const user=

useContext(

UserContext

);

const theme=

useContext(

ThemeContext

);

const language=

useContext(

LanguageContext

);
```

---

# Consumer Reading Functions

Provider

```jsx
<UserContext.Provider

value={{

user,

logout

}}

>
```

Consumer

```jsx
const{

user,

logout

}=

useContext(

UserContext

);

<button

onClick={logout}

>

Logout

</button>
```

Consumers can read both data and functions.

---

# Internal Working

```text
Provider

↓

Stores Value

↓

React Fiber

↓

Consumer Calls

↓

useContext()

↓

Current Context Value

↓

Component Re-renders
```

---

# What Happens When Provider Updates?

```text
Provider Value Changes

↓

React Detects Change

↓

Consumers Re-render

↓

Updated UI
```

Only components consuming that context are updated.

---

# Common Mistakes

## Using Consumer Without Provider

```jsx
const user=

useContext(

UserContext

);
```

If no matching Provider exists,

React returns

```
defaultValue
```

or

```
undefined
```

if no default value was provided.

---

## Using useContext Outside Component

Wrong

```jsx
const user=

useContext(

UserContext

);

function App(){}
```

Hooks must be called inside React function components or custom hooks.

---

## Using Context for Frequently Changing Data

Wrong

```
Mouse Position

Animation Frame

Typing Speed
```

These frequent updates can trigger many re-renders.

---

# Best Practices

- Prefer `useContext()` over `<Context.Consumer>`.
- Keep consumers close to where the data is needed.
- Split contexts if unrelated data changes independently.
- Memoize Provider values when appropriate.

---

# Interview Questions

## Beginner

### What is a Consumer?

A component that reads values from a Context Provider.

---

### Which Hook is used?

```jsx
useContext()
```

---

### Can Consumer read functions?

Yes.

Example

```
login()

logout()

setTheme()
```

---

# Intermediate

### Difference between Provider and Consumer?

| Provider | Consumer |
|-----------|----------|
| Shares Data | Reads Data |
| Uses `value` prop | Uses `useContext()` |
| Parent | Child |

---

### What if there is no Provider?

The Consumer receives the default value from `createContext(defaultValue)`, or `undefined` if no default value exists.

---

# Senior-Level Interview Questions

### Why is `useContext()` preferred over `Context.Consumer`?

- Simpler syntax.
- Better readability.
- Works naturally with Hooks.
- Easier to compose with other Hooks.

---

### Does every context update re-render consumers?

Yes.

Whenever the Provider's value changes, the components consuming that context are notified and re-render.

---

### How do you optimize Consumer performance?

- Split contexts by responsibility.
- Memoize Provider values.
- Avoid storing rapidly changing values in Context.
- Use selector-based libraries (Redux, Zustand) for complex state.

---

# Scenario-Based Questions

## Scenario 1

A component calls

```jsx
useContext(UserContext)
```

but the Provider is missing.

What happens?

**Answer**

The component receives the context's default value (or `undefined`).

---

## Scenario 2

Your Consumer re-renders every second.

Why?

**Answer**

The Provider's value changes every second, causing all consumers to update.

---

## Scenario 3

An interviewer asks:

**"Can one component consume multiple contexts?"**

**Answer**

Yes.

A component can call `useContext()` multiple times for different contexts.

---

## Scenario 4

Should you use `<Context.Consumer>` in modern React?

**Answer**

Generally no.

Use `useContext()` unless working with legacy code or class components.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Consumer | Reads Context |
| Modern API | `useContext()` |
| Legacy API | `<Context.Consumer>` |
| Reads | Data & Functions |
| Needs | Matching Provider |
| Without Provider | Default value / `undefined` |

---

# Excalidraw Diagram

```text
                 createContext()

                        │

                 UserContext

                        │

                Provider

                        │

      value = { user, logout }

                        │

      ┌─────────────────┼─────────────────┐

      │                 │                 │

   Header          Dashboard         Footer

      │                                   │

      └─────────────────┼─────────────────┘

                        │

                    Profile

                        │

         useContext(UserContext)

                        │

      user + logout Available

                        │

                Render UI
```

---

# Related Notes

- [[Context API]]
- [[Context API Provider]]
- [[useContext]]
- [[Prop Drilling]]
- [[Props]]
- [[State Management]]
- [[React Hooks]]
- [[Redux Toolkit]]
- [[Zustand]]