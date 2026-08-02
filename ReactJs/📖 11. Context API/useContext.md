---
title: useContext Hook in React
tags:
  - react
  - hooks
  - usecontext
  - context-api
  - interview
created: 2026-08-02
---

# useContext Hook in React

> [!INFO]
> **useContext()** is a built-in React Hook that allows a component to **read data from a Context Provider without passing props manually.**

> [!IMPORTANT]
> `useContext()` is the modern way to consume Context API and replaces the older `<Context.Consumer>` syntax.

---

# Beginner Explanation

Imagine an office.

The CEO publishes company announcements on the company portal.

```
CEO

↓

Company Portal

↓

Employees
```

Employees don't ask every manager for the information.

They simply open the portal.

Similarly,

```
Context Provider

↓

React Context

↓

useContext()

↓

Component
```

The component directly reads the shared data.

---

# Why use useContext?

Without useContext

```
App

↓

Header

↓

Navbar

↓

Sidebar

↓

Profile

↓

User
```

Every component passes props.

---

With useContext

```
App

↓

Provider

↓

React Context

↓

User Component
```

The User component gets the data directly.

---

# Syntax

```jsx
const value = useContext(ContextName);
```

---

# Parameters

```jsx
useContext(ContextObject)
```

The parameter must be the Context created using

```jsx
createContext()
```

---

# Return Value

Returns the current value from the nearest matching Provider.

Example

```jsx
const user = useContext(UserContext);
```

---

# Complete Flow

```text
createContext()

↓

Provider

↓

Stores Value

↓

useContext()

↓

Returns Value

↓

Render UI
```

---

# Step 1 — Create Context

```jsx
import { createContext } from "react";

export const UserContext = createContext();
```

---

# Step 2 — Provide Data

```jsx
import { UserContext } from "./UserContext";

function App() {

    const user = {
        name: "John",
        role: "Developer"
    };

    return (
        <UserContext.Provider value={user}>
            <Profile />
        </UserContext.Provider>
    );
}
```

---

# Step 3 — Read Data using useContext

```jsx
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Profile() {

    const user = useContext(UserContext);

    return (
        <>
            <h2>{user.name}</h2>
            <p>{user.role}</p>
        </>
    );
}
```

---

# Visual Flow

```text
            App

             │

      UserContext.Provider

             │

      value = User Object

             │

      React Context Store

             │

     useContext(UserContext)

             │

      Profile Component

             │

      Display User Details
```

---

# Reading Multiple Contexts

```jsx
const user = useContext(UserContext);

const theme = useContext(ThemeContext);

const language = useContext(LanguageContext);
```

---

# Reading Functions

Provider

```jsx
<UserContext.Provider
    value={{
        user,
        login,
        logout
    }}
>
```

Consumer

```jsx
const { login, logout } = useContext(UserContext);

<button onClick={logout}>
    Logout
</button>
```

Functions can also be shared.

---

# Updating Context

Provider

```jsx
const [theme, setTheme] = useState("light");

<UserContext.Provider
    value={{
        theme,
        setTheme
    }}
>
```

Consumer

```jsx
const { theme, setTheme } = useContext(UserContext);

<button
    onClick={() => setTheme("dark")}
>
    Dark Mode
</button>
```

---

# Internal Working

```text
Component Mount

↓

useContext()

↓

React Looks For Provider

↓

Provider Found

↓

Returns Current Value

↓

Render Component
```

---

# What Happens When Provider Updates?

```text
Provider Value Changes

↓

React Detects Change

↓

All Consumers

↓

Re-render

↓

Updated UI
```

---

# Real World Example

Authentication

```text
AuthProvider

↓

User

↓

Token

↓

Login()

↓

Logout()

↓

useContext(AuthContext)

↓

Navbar

Dashboard

Profile

Settings
```

---

# Common Mistakes

## Forgetting Provider

Wrong

```jsx
const user = useContext(UserContext);
```

Without Provider

Result

```
undefined
```

or

```
defaultValue
```

---

## Calling useContext Outside Component

Wrong

```jsx
const user = useContext(UserContext);

function App() {}
```

Hooks must be used only inside

- Functional Components
- Custom Hooks

---

## Using Wrong Context

Wrong

```jsx
useContext(ThemeContext);
```

Expected

```jsx
useContext(UserContext);
```

---

# Performance Considerations

When Provider value changes

```
Provider

↓

Every Consumer

↓

Re-render
```

Optimize using

- Split Contexts
- useMemo()
- Zustand
- Redux Toolkit

---

# Context vs useContext

| Context API | useContext |
|-------------|------------|
| Creates shared state | Reads shared state |
| Uses Provider | Uses Hook |
| `createContext()` | `useContext()` |

---

# useContext vs Props

| Props | useContext |
|--------|------------|
| Parent → Child | Any Component |
| Manual | Automatic |
| Small Apps | Shared Data |

---

# useContext vs Redux

| useContext | Redux Toolkit |
|-------------|---------------|
| Simple | Enterprise |
| Built into React | External Library |
| Small Apps | Large Apps |
| No Middleware | Middleware Support |
| No DevTools | Redux DevTools |

---

# Best Practices

- Use `useContext()` instead of `Context.Consumer`.
- Use it only for shared state.
- Keep Provider close to where it is needed.
- Memoize Provider values using `useMemo`.
- Split unrelated contexts.

---

# Interview Questions

## Beginner

### What is useContext?

A Hook used to consume data from a Context Provider.

---

### Which Hook reads Context?

```jsx
useContext()
```

---

### Does useContext replace Props?

No.

Props are still best for direct parent-child communication.

---

### Which Hook creates Context?

None.

Use

```jsx
createContext()
```

---

# Intermediate

### Does useContext cause re-render?

Yes.

Whenever the Provider's value changes, all components consuming that context are re-rendered.

---

### Can useContext read functions?

Yes.

Example

```
login()

logout()

setTheme()
```

---

### Can one component use multiple contexts?

Yes.

```jsx
const user = useContext(UserContext);
const theme = useContext(ThemeContext);
const lang = useContext(LanguageContext);
```

---

# Senior-Level Interview Questions

### Why isn't useContext a replacement for Redux?

Because `useContext()` only reads shared values.

Redux additionally provides:

- Predictable state updates
- Middleware
- DevTools
- Selectors
- Better scalability

---

### How do you optimize useContext performance?

- Split Contexts.
- Memoize Provider values.
- Avoid storing rapidly changing data.
- Use Zustand or Redux Toolkit for complex state.

---

### What happens if Provider is missing?

`useContext()` returns the default value supplied to `createContext(defaultValue)`. If no default value exists, it returns `undefined`.

---

# Scenario-Based Questions

## Scenario 1

Your application has

- Theme
- Language
- Logged-in User

Should you use useContext?

**Answer**

Yes.

These are ideal use cases for Context.

---

## Scenario 2

Your Provider updates every second.

What happens?

**Answer**

All components consuming that context re-render.

---

## Scenario 3

Should API responses be stored in Context?

**Answer**

Generally no.

Use TanStack Query or SWR for server state.

---

## Scenario 4

When would you choose Redux instead?

**Answer**

Large enterprise applications with complex global state, middleware requirements, and predictable state transitions.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Hook | `useContext()` |
| Reads | Context Data |
| Needs | Provider |
| Solves | Prop Drilling |
| Returns | Current Context Value |
| Large Apps | Redux Toolkit / Zustand |
| Server State | TanStack Query |

---

# Excalidraw Diagram

```text
                 createContext()

                        │

                  UserContext

                        │

                Provider

                        │

        value = { user, login }

                        │

             React Context Store

                        │

        ┌───────────────┼───────────────┐

        │               │               │

     Navbar         Dashboard       Profile

        │               │               │

        └───────────────┼───────────────┘

                        │

        useContext(UserContext)

                        │

      user + login + logout

                        │

                 Render UI
```

---

# Related Notes

- 