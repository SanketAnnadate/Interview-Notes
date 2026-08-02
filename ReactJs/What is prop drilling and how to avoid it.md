---
title: Prop Drilling in React
tags:
  - react
  - props
  - context-api
  - redux
  - zustand
  - state-management
  - interview
created: 2026-08-02
---

# What is Prop Drilling?

> [!INFO]
> **Prop Drilling** is the process of passing props through multiple intermediate components that **do not use the data themselves**, only to deliver it to a deeply nested child component.

> [!IMPORTANT]
> Prop drilling is **not a React bug**. It is a design problem that appears when many components need the same data.

---

# Beginner Explanation

Imagine your mother wants to send a message to your younger brother.

Instead of talking directly, the message goes through everyone.

```
Mother

↓

Father

↓

Sister

↓

You

↓

Brother
```

Although

```
Father

Sister

You
```

don't need the message,

they still pass it.

React has the same problem.

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

Only

```
User
```

needs the data,

but every component receives and forwards it.

This is **Prop Drilling**.

---

# Visual Representation

## Without Prop Drilling

```text
App

↓

User
```

---

## With Prop Drilling

```text
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

Every component passes the same prop.

---

# Example of Prop Drilling

```jsx
function App(){

const user="John";

return(

<Header

user={user}

/>

);

}

function Header({

user

}){

return(

<Navbar

user={user}

/>

);

}

function Navbar({

user

}){

return(

<Profile

user={user}

/>

);

}

function Profile({

user

}){

return(

<User

user={user}

/>

);

}

function User({

user

}){

return<h2>

{user}

</h2>;

}
```

---

# Data Flow

```text
App

↓

Header

↓

Navbar

↓

Profile

↓

User
```

Only

```
User
```

uses

```
user
```

The rest simply forward it.

---

# Why is Prop Drilling a Problem?

Imagine

```
50 Components

↓

10 Levels Deep

↓

20 Shared Props
```

Every component becomes cluttered with props it doesn't use.

Problems

- Hard to maintain
- Difficult to debug
- Large component interfaces
- Tight coupling
- Refactoring becomes risky

---

# How to Avoid Prop Drilling?

## 1. Context API ⭐

Instead of passing data through every component,

provide it once.

```text
App

↓

Context Provider

↓

Any Component
```

---

### Example

```jsx
const UserContext=

createContext();

function App(){

return(

<UserContext.Provider

value="John"

>

<Home/>

</UserContext.Provider>

);

}
```

Child

```jsx
import {

useContext

} from "react";

function Profile(){

const user=

useContext(

UserContext

);

return<h2>

{user}

</h2>;

}
```

No intermediate components receive props.

---

# Context Flow

```text
                 App

                  │

        UserContext.Provider

                  │

      ┌───────────┼───────────┐

      │           │           │

   Header      Sidebar     Footer

      │                       │

      └────────────┬──────────┘

                   │

              Any Component

                   │

          useContext(UserContext)
```

---

# 2. Redux Toolkit ⭐⭐⭐

Enterprise applications often use

```
Redux Toolkit
```

Flow

```text
Store

↓

Any Component

↓

useSelector()

↓

Data
```

Example

```jsx
const user=

useSelector(

state=>state.user

);
```

---

# 3. Zustand ⭐

Modern lightweight state management.

```jsx
const user=

useUserStore(

state=>state.user

);
```

---

# 4. TanStack Query

For **server state**.

Don't pass API data manually.

Instead

```
Component

↓

Query Cache

↓

Data
```

---

# Prop Drilling vs Context API

| Prop Drilling | Context API |
|---------------|-------------|
| Pass props manually | Share globally |
| Intermediate components required | No intermediate components |
| Difficult to maintain | Easy |
| Small apps | Medium apps |

---

# Context API vs Redux

| Context API | Redux Toolkit |
|--------------|---------------|
| Small/Medium apps | Large enterprise apps |
| Theme | Complex global state |
| Authentication | Dashboard |
| Language | Banking Apps |
| Simpler | More scalable |

---

# Internal Working

```text
App

↓

Props

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

↓

Render
```

With Context

```text
App

↓

Provider

↓

React Context

↓

Consumer

↓

Render
```

---

# Real-World Example

## Banking Application

Need logged-in user

Without Context

```
App

↓

Dashboard

↓

Header

↓

Menu

↓

Notification

↓

Profile
```

With Context

```
App

↓

UserContext

↓

Profile
```

Cleaner.

---

# Common Mistakes

## Using Context for Everything

Wrong.

Context causes all consumers to re-render when its value changes.

Use it for

- Theme
- Language
- Authentication
- User Preferences

Not for frequently changing large datasets.

---

## Deep Prop Chains

Wrong

```
15 Components

↓

Passing Same Prop
```

Better

```
Context

or

Redux
```

---

## Too Many Contexts

Wrong

```
30 Providers
```

Hard to manage.

Organize providers logically.

---

# Best Practices

- Use props for parent → child communication.
- Use Context for globally shared data.
- Use Redux Toolkit or Zustand for large applications.
- Don't prematurely optimize—prop drilling is acceptable for small component trees.
- Keep state close to where it is needed.

---

# Interview Questions

## Beginner

### What is Prop Drilling?

Passing props through multiple intermediate components to reach a deeply nested child.

---

### Why is it a problem?

Intermediate components receive props they don't use, increasing complexity.

---

### How can it be avoided?

- Context API
- Redux Toolkit
- Zustand
- Other state management libraries

---

# Intermediate

### Difference between Props and Context?

| Props | Context |
|--------|----------|
| Parent → Child | Any Component |
| Manual | Automatic |
| Local | Shared |

---

### Should Context replace Props?

No.

Props are still the best choice for direct parent-child communication.

Use Context only for shared global data.

---

# Senior-Level Interview Questions

### Does Context eliminate re-renders?

No.

When the context value changes, **all consuming components** that read that value re-render.

Optimization techniques include:

- Splitting contexts
- Memoization
- Selector-based state libraries (Redux, Zustand)

---

### When should Redux be preferred?

When applications require:

- Large global state
- Predictable updates
- Middleware
- DevTools
- Complex business logic

---

### When is prop drilling acceptable?

- Small applications
- 1–2 component levels
- Data used by only a few components

Avoid introducing Context or Redux unnecessarily.

---

# Scenario-Based Questions

## Scenario 1

A theme value is passed through 10 components.

How would you improve it?

**Answer**

Use Context API.

---

## Scenario 2

A banking dashboard has:

- User
- Accounts
- Transactions
- Notifications
- Permissions

Which solution?

**Answer**

Redux Toolkit.

---

## Scenario 3

Only one child needs a prop.

Should Context be used?

**Answer**

No.

Simply pass the prop.

---

## Scenario 4

An interviewer asks:

**"Is prop drilling always bad?"**

**Answer**

No.

Prop drilling is perfectly acceptable for shallow component trees.

Introducing Context or Redux too early can make an application more complex than necessary.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Prop Drilling | Passing props through many components |
| Problem | Intermediate components don't use the data |
| Small Apps | Props |
| Medium Apps | Context API |
| Large Apps | Redux Toolkit / Zustand |
| API Data | TanStack Query |

---

# Excalidraw Diagram

```text
                    Prop Drilling

                         │

                         ▼

                       App

                         │

                    user="John"

                         │

                     Header

                         │

                     Navbar

                         │

                     Sidebar

                         │

                     Profile

                         │

                       User

                         │

                    Uses Data


──────────────────────────────────────────────

                  Better Solution

                         │

                        App

                         │

              UserContext.Provider

                         │

        ┌────────────────┼────────────────┐

        │                │                │

     Header          Navbar         Sidebar

        │                │                │

        └────────────────┼────────────────┘

                         │

                     Any Component

                         │

          useContext(UserContext)

                         │

                    Gets Data Directly
```

---

# Related Notes

- 