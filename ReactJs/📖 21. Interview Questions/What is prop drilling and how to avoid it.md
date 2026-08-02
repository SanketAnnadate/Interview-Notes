---
title: Prop Drilling in React
tags:
  - react
  - props
  - context-api
  - redux
  - zustand
  - interview
---

# What is Prop Drilling and How to Avoid It?

> [!INFO]
> **Prop Drilling** is the process of passing data (props) through multiple intermediate components that do **not use the data themselves**, only to pass it to a deeply nested child component.

> [!IMPORTANT]
> Prop Drilling is **not a React problem**. It is a **component design problem**. It becomes an issue when many nested components pass the same props without using them.

---

# Beginner Explanation

Imagine a family.

```
Grandfather

↓

Father

↓

Brother

↓

You
```

Grandfather wants to give a message only to **You**.

However,

```
Father

Brother
```

don't need the message.

They simply forward it.

This unnecessary forwarding is called **Prop Drilling**.

React works the same way.

---

# React Example

Suppose we have the following component hierarchy:

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

Only the **User** component needs the logged-in user's name.

Instead of accessing it directly,

every component forwards the prop.

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

This is **Prop Drilling**.

---

# Example Without Prop Drilling

```
App

↓

User
```

Simple and easy.

---

# Example With Prop Drilling

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

Five unnecessary components pass the same prop.

---

# Example Code

## App.jsx

```jsx
function App() {

    const user = "John";

    return <Header user={user} />;
}
```

---

## Header.jsx

```jsx
function Header({ user }) {

    return <Navbar user={user} />;
}
```

---

## Navbar.jsx

```jsx
function Navbar({ user }) {

    return <Sidebar user={user} />;
}
```

---

## Sidebar.jsx

```jsx
function Sidebar({ user }) {

    return <Profile user={user} />;
}
```

---

## Profile.jsx

```jsx
function Profile({ user }) {

    return <User user={user} />;
}
```

---

## User.jsx

```jsx
function User({ user }) {

    return <h2>Welcome {user}</h2>;
}
```

---

# Why is Prop Drilling a Problem?

### Unnecessary Props

```
Header

↓

Navbar

↓

Sidebar
```

receive props they never use.

---

### Difficult Maintenance

If a prop changes,

multiple components must also change.

---

### Tight Coupling

Intermediate components become dependent on props they don't need.

---

### Poor Scalability

Imagine

```
30 Components

↓

12 Levels

↓

20 Shared Props
```

The application becomes difficult to maintain.

---

# How to Avoid Prop Drilling?

## Solution 1 — Context API ⭐⭐⭐⭐

Store shared data inside Context.

Instead of

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

Use

```
UserContext.Provider

↓

Any Component

↓

useContext()
```

Example

```jsx
const UserContext = createContext();

<UserContext.Provider value={user}>
    <App />
</UserContext.Provider>
```

Consumer

```jsx
const user = useContext(UserContext);
```

No intermediate props.

---

## Solution 2 — Redux Toolkit ⭐⭐⭐⭐⭐

Large enterprise applications usually use Redux Toolkit.

```
Redux Store

↓

Any Component

↓

useSelector()

↓

Data
```

Example

```jsx
const user = useSelector(state => state.user);
```

---

## Solution 3 — Zustand ⭐⭐⭐⭐

Lightweight global state management.

```jsx
const user = useUserStore(state => state.user);
```

---

## Solution 4 — Component Composition

Sometimes Context isn't required.

Instead of passing props,

pass components.

```jsx
function Layout({ children }) {

    return (
        <div>
            {children}
        </div>
    );
}
```

Usage

```jsx
<Layout>

    <User />

</Layout>
```

---

# Internal Working

## Prop Drilling

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

↓

Render
```

---

## Context API

```text
Provider

↓

React Context

↓

useContext()

↓

User

↓

Render
```

---

# When Should You Use Each Solution?

| Situation | Recommended Solution |
|-----------|----------------------|
| Parent → Child | Props |
| Shared Theme | Context API |
| Authentication | Context API |
| Shopping Cart | Context API / Zustand |
| Banking Dashboard | Redux Toolkit |
| API Data | TanStack Query |

---

# Real World Example

### Banking Application

Shared data:

- Logged-in User
- Permissions
- Theme
- Notifications

Wrong

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

Correct

```
AuthContext

↓

Any Component
```

or

```
Redux Store

↓

Any Component
```

---

# Common Mistakes

## Passing Everything as Props

Wrong

```jsx
<App

user

theme

language

permissions

cart

notifications

settings

/>;
```

---

## Using Context for Everything

Context is excellent for shared state, but not every piece of data belongs there.

For direct parent → child communication, use props.

---

## Huge Context Object

Avoid

```
User

Theme

Language

Cart

Notifications

Permissions
```

inside a single Context.

Create separate contexts instead.

---

# Best Practices

- Use props for direct parent → child communication.
- Use Context API for shared application state.
- Use Redux Toolkit or Zustand for large applications.
- Keep state close to where it's needed.
- Don't introduce Context if prop drilling is only one or two levels deep.

---

# Interview Questions

## Beginner

### What is Prop Drilling?

Passing props through multiple intermediate components to reach a deeply nested child.

---

### Why is it a problem?

Intermediate components receive props they don't use, increasing complexity.

---

### How can you avoid Prop Drilling?

- Context API
- Redux Toolkit
- Zustand
- Component Composition

---

## Intermediate

### Does Context API replace Props?

No.

Props are still the preferred way to communicate between a parent and its direct child.

---

### When should Context API be used?

When multiple unrelated components at different levels need the same shared data.

---

## Senior-Level Interview Questions

### Is Prop Drilling always bad?

No.

If data is passed through only one or two levels, using props is simpler than introducing Context.

---

### Why isn't Context API always the best solution?

Every time the Provider's value changes, all components consuming that context re-render.

For large applications, Redux Toolkit or Zustand may provide better scalability and performance.

---

### How do large companies solve Prop Drilling?

- Context API (Theme, Authentication, Language)
- Redux Toolkit (Complex global state)
- Zustand (Lightweight state management)
- TanStack Query (Server state)

---

# Scenario-Based Questions

## Scenario 1

The logged-in user is passed through 8 components.

What would you do?

**Answer**

Move the user data into `UserContext` or Redux.

---

## Scenario 2

Only one child component needs the data.

Should Context API be used?

**Answer**

No.

Simply pass props.

---

## Scenario 3

A Theme value is used across the application.

Which solution?

**Answer**

Context API.

---

## Scenario 4

A banking application manages:

- Accounts
- Loans
- Cards
- Investments
- Notifications

Which solution?

**Answer**

Redux Toolkit for client state and TanStack Query for API/server state.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Problem | Passing props through many components |
| Small Apps | Props |
| Shared State | Context API |
| Large Apps | Redux Toolkit / Zustand |
| Server State | TanStack Query |
| Alternative | Component Composition |

---

# Excalidraw Diagram

```text
                PROP DRILLING

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

               (Uses the data)


────────────────────────────────────

           CONTEXT API SOLUTION

                     App

                      │

        UserContext.Provider

                      │

        ┌─────────────┼─────────────┐

        │             │             │

     Header       Navbar       Sidebar

        │                           │

        └─────────────┼─────────────┘

                      │

                     User

                      │

     useContext(UserContext)

                      │

              Gets User Directly


────────────────────────────────────

          REDUX TOOLKIT SOLUTION

                Redux Store

                      │

         useSelector(state.user)

                      │

             Any Component

                      │

               Gets User Data
```

---

# Related Notes

- [[Props]]
- [[Context API]]
- [[Provider]]
- [[Consumer]]
- [[useContext]]
- 