---
title: Multiple Contexts in React
tags:
  - react
  - context-api
  - usecontext
  - multiple-contexts
  - interview
created: 2026-08-02
---

# How do you use Multiple Contexts in a Single Component?

> [!INFO]
> React allows a component to consume **multiple Contexts** by calling the `useContext()` hook multiple times.

> [!IMPORTANT]
> Large enterprise applications rarely have just one Context. Instead, they split application state into multiple Contexts such as:
>
> - Authentication
> - Theme
> - Language
> - Cart
> - Notification
> - Settings

---

# Beginner Explanation

Imagine your mobile phone.

It gets data from different services.

```
Internet

↓

WiFi
```

```
Time

↓

Network
```

```
Theme

↓

Settings
```

```
Language

↓

System Settings
```

Although all data comes from different places,

your phone displays everything together.

React works the same way.

One component can read data from multiple Context Providers.

---

# Why Multiple Contexts?

Instead of

```
One Huge Context

↓

User

Theme

Language

Cart

Notification

Settings

Permissions
```

We split them.

```
UserContext

ThemeContext

LanguageContext

CartContext
```

This makes the application cleaner and easier to maintain.

---

# Visual Representation

```text
                     App

                      │

          ThemeProvider

                      │

          UserProvider

                      │

       LanguageProvider

                      │

          CartProvider

                      │

               Dashboard
```

Dashboard can access **all four contexts**.

---

# Creating Multiple Contexts

```jsx
import {

createContext

} from "react";

export const UserContext=

createContext();

export const ThemeContext=

createContext();

export const LanguageContext=

createContext();
```

---

# Providing Multiple Contexts

```jsx
function App(){

return(

<UserContext.Provider

value={{

name:"John"

}}

>

<ThemeContext.Provider

value="dark"

>

<LanguageContext.Provider

value="English"

>

<Home/>

</LanguageContext.Provider>

</ThemeContext.Provider>

</UserContext.Provider>

);

}
```

---

# Reading Multiple Contexts

```jsx
import {

useContext

} from "react";

function Home(){

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

return(

<>

<h2>

{user.name}

</h2>

<p>

{theme}

</p>

<p>

{language}

</p>

</>

);

}
```

---

# Data Flow

```text
UserContext

↓

ThemeContext

↓

LanguageContext

↓

Home Component

↓

useContext()

↓

User

Theme

Language
```

---

# Real Enterprise Example

A Banking Dashboard may use

```text
AuthContext

↓

ThemeContext

↓

NotificationContext

↓

PermissionContext

↓

Dashboard
```

Dashboard can access

```
Logged-in User

Theme

Unread Notifications

User Permissions
```

without prop drilling.

---

# Using Custom Providers

Instead of nesting Providers in `App.jsx`, create a wrapper component.

## AppProviders.jsx

```jsx
export function AppProviders({

children

}){

return(

<AuthProvider>

<ThemeProvider>

<LanguageProvider>

<CartProvider>

{children}

</CartProvider>

</LanguageProvider>

</ThemeProvider>

</AuthProvider>

);

}
```

---

## App.jsx

```jsx
import {

AppProviders

} from "./AppProviders";

function App(){

return(

<AppProviders>

<Home/>

</AppProviders>

);

}
```

This keeps `App.jsx` clean.

---

# Internal Working

```text
App

↓

User Provider

↓

Theme Provider

↓

Language Provider

↓

React Context Store

↓

Component

↓

useContext()

↓

Gets All Values
```

---

# How React Finds Context

Suppose

```jsx
const theme=

useContext(

ThemeContext

);
```

React searches **up the component tree** until it finds the nearest matching `ThemeContext.Provider`.

```text
Profile

↑

Dashboard

↑

ThemeProvider

✔ Found
```

The nearest Provider wins.

---

# Common Mistakes

## One Huge Context

Wrong

```text
User

Theme

Language

Cart

Notification

Settings

Permissions
```

All inside one Context.

---

Better

```
UserContext

ThemeContext

CartContext

LanguageContext
```

---

## Deep Provider Nesting

```jsx
<UserProvider>

<ThemeProvider>

<CartProvider>

<LanguageProvider>

<PermissionProvider>

<App/>

</PermissionProvider>

</LanguageProvider>

</CartProvider>

</ThemeProvider>

</UserProvider>
```

Create a reusable `AppProviders` component to improve readability.

---

## Frequent Updates

Avoid storing rapidly changing values (e.g., mouse position or animation frames) in Context because every update causes consuming components to re-render.

---

# Best Practices

- Create one Context per responsibility.
- Use multiple `useContext()` calls instead of combining unrelated state.
- Keep Providers close to where they are needed.
- Group Providers inside an `AppProviders` component.
- Memoize Provider values when appropriate.

---

# Performance Considerations

Instead of

```jsx
value={{

user,

theme

}}
```

use

```jsx
const value=

useMemo(

()=>({

user,

theme

}),

[user,theme]

);
```

This prevents unnecessary re-renders caused by creating a new object every render.

---

# Interview Questions

## Beginner

### Can a component use multiple Contexts?

Yes.

Call `useContext()` once for each Context.

---

### How many Contexts can a component consume?

There is no fixed limit.

---

### Do Contexts interfere with each other?

No.

Each Context is independent.

---

# Intermediate

### Why split Contexts?

- Better performance
- Easier maintenance
- Cleaner architecture
- Smaller re-render scope

---

### Which Provider does React use?

The **nearest matching Provider** in the component tree.

---

# Senior-Level Interview Questions

### Why shouldn't everything be placed in one Context?

Because every update to the Provider can cause all consuming components to re-render, even if they only need one part of the data.

---

### How do enterprise applications organize Providers?

Typically with a wrapper component such as:

```text
AppProviders

├── AuthProvider

├── ThemeProvider

├── LanguageProvider

├── NotificationProvider

└── CartProvider
```

---

### When would you replace multiple Contexts with Redux or Zustand?

When the application has:

- Complex shared state
- Frequent updates
- Advanced state logic
- Need for middleware or DevTools

---

# Scenario-Based Questions

## Scenario 1

Your application has:

- Theme
- Logged-in User
- Language
- Shopping Cart

How would you organize it?

**Answer**

Create four separate Contexts:

- ThemeContext
- UserContext
- LanguageContext
- CartContext

---

## Scenario 2

Your Dashboard needs data from all four contexts.

How do you access them?

**Answer**

Call `useContext()` once for each Context.

---

## Scenario 3

An interviewer asks:

**"If there are two ThemeProviders, which value is used?"**

**Answer**

React always uses the **nearest ThemeProvider** above the consuming component.

---

## Scenario 4

A Theme update causes many components to re-render.

How can you optimize it?

**Answer**

- Split contexts by responsibility.
- Memoize Provider values.
- Scope Providers more narrowly.
- Consider selector-based state libraries for large applications.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Multiple Contexts | One component can consume many contexts |
| Hook | `useContext()` |
| Pattern | Separate Context per responsibility |
| Nearest Provider | React uses the closest matching Provider |
| Enterprise | Wrap Providers in `AppProviders` |
| Large Apps | Redux Toolkit / Zustand |

---

# Excalidraw Diagram

```text
                      App

                       │

                 AppProviders

                       │

        ┌──────────────┼──────────────┐

        │              │              │

   UserProvider   ThemeProvider   LanguageProvider

                       │

                 CartProvider

                       │

                  Dashboard

                       │

      ┌────────┬────────┬────────┐

      │        │        │        │

 useContext  useContext useContext useContext

(User)      (Theme)   (Language) (Cart)

      │        │        │        │

      └────────┴────────┴────────┘

               Render UI
```

---

# Related Notes

- [[Context API]]
- [[Context API Provider]]
- [[Context API Consumer]]
- [[useContext]]
- [[Prop Drilling]]
- [[State Management]]
- [[React Hooks]]
- [[Redux Toolkit]]
- [[Zustand]]
- [[Performance Optimization]]