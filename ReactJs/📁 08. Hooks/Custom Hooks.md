---
title: Custom Hooks
tags:
  - react
  - hooks
  - custom-hooks
  - interview
created: 2026-08-04
---

# Custom Hooks in React

> [!INFO]
> A **Custom Hook** is a JavaScript function that starts with the word **use** and allows you to **extract and reuse stateful logic** across multiple React components.

> **Rule:** Custom Hooks **share logic, NOT state**.

---

# Beginner Explanation

Suppose multiple components need to

- Fetch data
- Detect window size
- Manage authentication
- Handle forms

Instead of writing the same logic repeatedly, create a **Custom Hook**.

```
Component A

↓

Fetch Logic

Component B

↓

Fetch Logic

Component C

↓

Fetch Logic
```

After Custom Hook

```
Component A

        ↘

Component B ----> useFetch()

        ↗

Component C
```

Logic is written only once.

---

# Why Do We Need Custom Hooks?

Without Custom Hooks

```
Component A

↓

100 lines

----------------

Component B

↓

100 lines

----------------

Component C

↓

100 lines
```

Lots of duplicated code.

With Custom Hook

```
Component A

↓

useFetch()

Component B

↓

useFetch()

Component C

↓

useFetch()
```

One reusable implementation.

---

# Definition

A Custom Hook is

- A normal JavaScript function
- Name must start with **use**
- Can call other hooks
- Returns reusable logic

Example

```jsx
function useCounter(){

}
```

Correct

```jsx
useFetch()

useAuth()

useTheme()

useWindowSize()
```

Wrong

```jsx
fetchHook()

counterHook()

myHook()
```

---

# Syntax

```jsx
function useSomething(){

// Hooks

// Logic

// Return values

}
```

---

# Simple Counter Hook

```jsx
import { useState } from "react";

function useCounter(){

const [count,setCount]=useState(0);

function increment(){

setCount(c=>c+1);

}

function decrement(){

setCount(c=>c-1);

}

return{

count,

increment,

decrement

};

}
```

---

# Using Custom Hook

```jsx
function Counter(){

const {

count,

increment,

decrement

}=useCounter();

return(

<>

<h1>{count}</h1>

<button onClick={increment}>

+

</button>

<button onClick={decrement}>

-

</button>

</>

);

}
```

---

# Working Flow

```
Component

↓

useCounter()

↓

useState()

↓

Returns

count

increment

decrement

↓

Component Uses Them
```

---

# Custom Hook for Data Fetching

```jsx
import { useState,useEffect } from "react";

function useFetch(url){

const [data,setData]=useState(null);

const [loading,setLoading]=useState(true);

const [error,setError]=useState(null);

useEffect(()=>{

async function fetchData(){

try{

setLoading(true);

const response=await fetch(url);

const result=await response.json();

setData(result);

}

catch(err){

setError(err);

}

finally{

setLoading(false);

}

}

fetchData();

},[url]);

return{

data,

loading,

error

};

}
```

---

# Using useFetch()

```jsx
function Users(){

const {

data,

loading,

error

}=useFetch("/users");

if(loading)

return <h2>Loading...</h2>;

if(error)

return <h2>Error</h2>;

return(

<div>

{JSON.stringify(data)}

</div>

);

}
```

---

# Custom Hook for Window Size

```jsx
import { useState,useEffect } from "react";

function useWindowSize(){

const [width,setWidth]=useState(window.innerWidth);

useEffect(()=>{

function resize(){

setWidth(window.innerWidth);

}

window.addEventListener("resize",resize);

return()=>{

window.removeEventListener("resize",resize);

};

},[]);

return width;

}
```

---

# Usage

```jsx
function App(){

const width=useWindowSize();

return(

<h2>

Width : {width}

</h2>

);

}
```

---

# Custom Hook for Local Storage

```jsx
function useLocalStorage(key,initialValue){

const [value,setValue]=useState(()=>{

const item=localStorage.getItem(key);

return item

? JSON.parse(item)

: initialValue;

});

useEffect(()=>{

localStorage.setItem(

key,

JSON.stringify(value)

);

},[key,value]);

return [value,setValue];

}
```

---

# Usage

```jsx
const [theme,setTheme]

=

useLocalStorage(

"theme",

"light"

);
```

---

# Custom Hook for Debounce

```jsx
function useDebounce(value,delay){

const [debounced,setDebounced]

=

useState(value);

useEffect(()=>{

const timer=

setTimeout(()=>{

setDebounced(value);

},delay);

return()=>{

clearTimeout(timer);

};

},[value,delay]);

return debounced;

}
```

---

# Usage

```jsx
const search

=

useDebounce(text,500);
```

Perfect for

- Search bars
- API calls
- Autocomplete

---

# Custom Hook Folder Structure

```
src

│

├── hooks

│     │

│     ├── useFetch.js

│     ├── useCounter.js

│     ├── useLocalStorage.js

│     ├── useDebounce.js

│     ├── useWindowSize.js

│     └── useAuth.js

│

├── components

├── pages

└── App.js
```

---

# Benefits

✅ Reusable

✅ Cleaner Components

✅ Better Maintainability

✅ Easier Testing

✅ Separation of Concerns

✅ Less Duplicate Code

---

# Custom Hook vs Utility Function

| Custom Hook | Utility Function |
|-------------|------------------|
| Uses React Hooks | Cannot use Hooks |
| Has state | No state |
| Uses useEffect | No lifecycle |
| React-specific | Plain JavaScript |

---

# Custom Hook vs Component

| Component | Custom Hook |
|------------|------------|
| Returns JSX | Returns logic |
| Renders UI | No UI |
| Used in JSX | Called like a hook |

---

# Common Mistakes

### Wrong

```jsx
function fetchData(){

useState();

}
```

Hook name doesn't start with **use**.

---

### Correct

```jsx
function useFetch(){

useState();

}
```

---

### Wrong

Calling inside condition

```jsx
if(user){

useFetch();

}
```

---

### Correct

```jsx
const users=useFetch();
```

---

# Rules for Custom Hooks

- Must start with **use**
- Only call hooks at the top level
- Never call hooks inside loops
- Never call hooks inside conditions
- Never call hooks inside nested functions
- Can call other hooks

---

# Real-world Examples

Large companies commonly create hooks such as:

```
useApi()

useAuth()

useAxios()

usePermissions()

useTheme()

useDarkMode()

useInfiniteScroll()

usePagination()

useSocket()

useDebounce()

useThrottle()

useClipboard()

useOnlineStatus()

useCurrentLocation()

useMediaQuery()

usePrevious()

useLocalStorage()

useSessionStorage()
```

---

# Performance Tips

- Return only necessary values.
- Memoize callbacks with `useCallback` when appropriate.
- Memoize expensive derived values with `useMemo`.
- Avoid unnecessary state inside hooks.
- Clean up subscriptions and event listeners.

---

# Interview Questions

## Beginner

### What is a Custom Hook?

A reusable function that starts with **use** and encapsulates reusable React stateful logic.

---

### Why use Custom Hooks?

To eliminate duplicate logic and keep components clean.

---

### Why must a Custom Hook start with **use**?

React uses the naming convention to enforce the Rules of Hooks through its linter and tooling.

---

## Intermediate

### Can Custom Hooks share state?

No.

Each call creates its own independent state.

```
Component A

↓

useCounter()

↓

count = 1

-------------------

Component B

↓

useCounter()

↓

count = 0
```

They share **logic**, not **state**.

---

### Can a Custom Hook call another Custom Hook?

Yes.

Example

```
useAuth()

↓

useFetch()

↓

useLocalStorage()
```

---

## Senior-Level Questions

### How would you design reusable hooks in a large application?

- Keep them focused on one responsibility.
- Accept configuration through parameters.
- Return only the required API.
- Handle loading and error states.
- Clean up side effects properly.
- Write unit tests for hooks.

---

### When should you NOT create a Custom Hook?

Avoid creating one if the logic is used only once or if extracting it makes the code harder to understand.

---

# Scenario-Based Questions

## Scenario 1

Three pages call the same API.

**Answer**

Create a `useFetch()` hook.

---

## Scenario 2

Many components need the current window width.

**Answer**

Create a `useWindowSize()` hook.

---

## Scenario 3

A search input fires an API request on every keypress.

**Answer**

Use a `useDebounce()` hook to delay requests until typing stops.

---

## Scenario 4

The interviewer asks:

**Can two components using the same Custom Hook share state?**

**Answer**

No. Every invocation has its own isolated state. To share state across components, use Context, Redux, Zustand, or another shared state solution.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Starts With | `use` |
| Returns | Logic, state, functions |
| Shares | Logic |
| Doesn't Share | State |
| Can Use Hooks | ✅ |
| Can Return JSX | ❌ |
| Used For | Reusable stateful logic |

---

# Excalidraw Mind Map

```text
                    Custom Hooks

                          │

        ┌─────────────────┼──────────────────┐

        │                 │                  │

   Reusable Logic     Uses Hooks       No JSX Returned

        │                 │                  │

   useState()       useEffect()        Returns Data

        │

        ├───────────────┐

        │               │

   useFetch()      useCounter()

        │               │

  Loading/Error     Increment

        │               │

   useWindowSize()  useLocalStorage()

        │

     useDebounce()

        │

     Cleaner Components

        │

    Less Duplicate Code

        │

   Better Maintainability
```

---

# Related Notes

- [[Hooks]]
```