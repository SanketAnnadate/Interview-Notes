---
title: Context API in React
tags:
  - react
  - context-api
  - usecontext
  - state-management
  - prop-drilling
  - interview
created: 2026-08-02
---

# What is the Context API in React?

> [!INFO]
> **Context API** is a built-in React feature that allows you to **share data across multiple components without manually passing props through every intermediate component.**

> [!IMPORTANT]
> Context API is mainly used to solve **Prop Drilling**.

---

# Beginner Explanation

Imagine a company.

Without Context API

```
CEO

↓

Manager

↓

Team Lead

↓

Senior

↓

Junior
```

If the CEO wants to send one message to the Junior,

everyone has to forward it.

This is

```
Prop Drilling
```

---

With Context API

```
CEO

↓

Company Portal

↓

Anyone can read it
```

React Context works exactly like this.

---

# Why was Context API introduced?

Without Context

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

Every component receives

```
user
```

even though only

```
User
```

needs it.

---

With Context

```
App

↓

Context Provider

↓

User Component
```

No unnecessary props.

---

# Real World Use Cases

Context API is commonly used for

- Logged-in User
- Authentication
- Theme (Dark/Light)
- Language
- Currency
- Permissions
- Settings
- Shopping Cart Count

---

# How Context API Works

React Context has **three parts**

```
createContext()

↓

Provider

↓

Consumer (useContext)
```

---

# Step 1 — Create Context

```jsx
import {

createContext

} from "react";

export const UserContext=

createContext();
```

React creates a global container.

---

# Step 2 — Provide Data

```jsx
import {

UserContext

} from "./UserContext";

function App(){

const user={

name:"John",

role:"Admin"

};

return(

<UserContext.Provider

value={user}

>

<Home/>

</UserContext.Provider>

);

}
```

Everything inside

```
Provider
```

can access the value.

---

# Step 3 — Consume Data

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

No props needed.

---

# Complete Flow

```text
createContext()

↓

Provider

↓

React Context

↓

useContext()

↓

Component Gets Data
```

---

# Visual Diagram

```text
                    App

                     │

        UserContext.Provider

                     │

     ┌───────────────┼───────────────┐

     │               │               │

 Header          Dashboard        Footer

     │                               │

     └───────────────┼───────────────┘

                     │

                  Profile

                     │

      useContext(UserContext)

                     │

               Gets User Data
```

---

# Complete Example

### UserContext.js

```jsx
import {

createContext

} from "react";

export const UserContext=

createContext();
```

---

### App.jsx

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

export default App;
```

---

### Profile.jsx

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

# Internal Working

```text
App

↓

Provider

↓

React Fiber

↓

Context Value Stored

↓

Consumer

↓

useContext()

↓

Component Re-render
```

---

# Updating Context

Context can also store functions.

```jsx
const[

theme,

setTheme

]=useState("light");

<UserContext.Provider

value={{

theme,

setTheme

}}

>
```

Child

```jsx
const{

theme,

setTheme

}=

useContext(

UserContext

);

<button

onClick={()=>

setTheme(

"dark"

)

}

>

Dark

</button>
```

Now any component can change the theme.

---

# Context with Authentication

```jsx
const AuthContext=

createContext();
```

Provider

```jsx
<AuthContext.Provider

value={{

user,

login,

logout

}}

>

<App/>

</AuthContext.Provider>
```

Anywhere

```jsx
const{

user,

logout

}=

useContext(

AuthContext

);
```

---

# Multiple Contexts

Large applications use multiple contexts.

```text
App

│

├── ThemeContext

├── UserContext

├── LanguageContext

├── NotificationContext

└── SettingsContext
```

---

# Context vs Props

| Props | Context |
|--------|----------|
| Parent → Child | Any Component |
| Manual Passing | Automatic Access |
| Small Data | Shared Data |
| Simple | Global |

---

# Context vs Redux

| Context API | Redux Toolkit |
|--------------|---------------|
| Built into React | External Library |
| Small-Medium Apps | Enterprise Apps |
| Simple | Powerful |
| No Middleware | Middleware Support |
| No DevTools | Redux DevTools |

---

# Context vs Zustand

| Context | Zustand |
|----------|----------|
| React Built-in | External |
| Re-renders Consumers | Selector-based updates |
| Small Apps | Medium-Large Apps |

---

# Common Mistakes

## Using Context for Everything

Wrong.

```
100 API Responses

↓

Context
```

This causes unnecessary re-renders.

---

Correct

Context should hold

- User
- Theme
- Auth
- Language

---

## Huge Context Object

Wrong

```
50 Values

↓

One Context
```

Split them.

```
ThemeContext

UserContext

LanguageContext
```

---

## Updating Context Frequently

Avoid storing rapidly changing values like mouse position or animation frames in Context.

---

# Best Practices

- Use Context for shared global state.
- Split large contexts into smaller ones.
- Keep Provider close to where it's needed.
- Memoize context values when appropriate to avoid unnecessary re-renders.
- Use Redux Toolkit, Zustand, or TanStack Query when Context is no longer sufficient.

---

# Performance Optimization

Wrong

```jsx
<UserContext.Provider

value={{

user,

theme

}}

>
```

A new object is created every render.

---

Better

```jsx
const value=

useMemo(

()=>({

user,

theme

}),

[user,theme]

);

<UserContext.Provider

value={value}

>
```

---

# Interview Questions

## Beginner

### What is Context API?

A built-in React feature used to share data across components without prop drilling.

---

### Which Hook consumes Context?

```jsx
useContext()
```

---

### Which function creates Context?

```jsx
createContext()
```

---

### Why use Context?

To avoid prop drilling.

---

# Intermediate

### Explain Context flow.

```
createContext()

↓

Provider

↓

useContext()

↓

Consumer
```

---

### Can Context store functions?

Yes.

Example

```
login()

logout()

setTheme()
```

---

### Can we have multiple Providers?

Yes.

Large applications commonly use multiple contexts.

---

# Senior-Level Interview Questions

### Does Context replace Redux?

No.

Context is a dependency injection mechanism for shared values.

Redux is a full state management solution with predictable updates, middleware, DevTools, selectors, and scalability.

---

### Does Context improve performance?

Not necessarily.

Whenever the Provider's value changes, all consuming components that use that context will re-render.

---

### How do you optimize Context?

- Split contexts.
- Memoize provider values with `useMemo`.
- Keep providers narrow in scope.
- Use selector-based state libraries for large applications.

---

### Why isn't Context recommended for frequently changing state?

Because every context update notifies all consumers, which can cause unnecessary re-renders if not structured carefully.

---

# Scenario-Based Questions

## Scenario 1

An application has

```
Theme

Language

Authentication
```

Which solution?

**Answer**

Context API.

---

## Scenario 2

A banking application has

- Accounts
- Transactions
- Investments
- Notifications
- Loans
- Cards

Should Context be used?

**Answer**

Prefer Redux Toolkit or Zustand because the state is complex and frequently updated.

---

## Scenario 3

An interviewer asks

**"When would you NOT use Context?"**

**Answer**

When managing:

- Large global state
- Frequently changing state
- Complex business logic
- Server state (use TanStack Query)

---

## Scenario 4

Your Context Provider causes the whole application to re-render.

How do you fix it?

**Answer**

- Split contexts by responsibility.
- Memoize provider values.
- Move providers closer to the components that need them.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| API | Context API |
| Create | `createContext()` |
| Share | `Provider` |
| Read | `useContext()` |
| Solves | Prop Drilling |
| Good For | Theme, Auth, Language |
| Large Apps | Redux Toolkit / Zustand |
| Server Data | TanStack Query |

---

# Excalidraw Diagram

```text
                Context API

                     │

          createContext()

                     │

          UserContext Object

                     │

             Provider

                     │

      ┌──────────────┼──────────────┐

      │              │              │

   Header         Dashboard      Footer

      │              │              │

      └──────────────┼──────────────┘

                     │

                 Profile

                     │

         useContext(UserContext)

                     │

             User Information


──────────────────────────────────────────────

Without Context

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


──────────────────────────────────────────────

With Context

App

↓

Provider

↓

React Context

↓

Any Component
```

---

# Related Notes

- 