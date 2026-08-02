---
title: Data Fetching in React
tags:
  - react
  - useeffect
  - fetch
  - axios
  - api
  - async
  - interview
created: 2026-08-02
---

# How to Implement Data Fetching in React?

> [!INFO]
> **Data Fetching** is the process of requesting data from a server or API and displaying it in a React application.

> [!IMPORTANT]
> In modern React, data fetching is usually done using:
>
> - `fetch()` (Built-in Browser API)
> - `Axios`
> - `TanStack Query (React Query)` ⭐
> - `SWR`
>
> API calls are typically placed inside `useEffect()` or managed by specialized data-fetching libraries.

---

# Beginner Explanation

Imagine ordering food online.

```
You Place Order

↓

Restaurant Receives Order

↓

Food Prepared

↓

Delivery

↓

You Eat
```

React follows a similar process.

```
Component Loads

↓

API Request

↓

Server Response

↓

Store in State

↓

Display UI
```

---

# Data Fetching Flow

```text
React Component

       │

       ▼

useEffect()

       │

       ▼

API Request

       │

       ▼

Server

       │

       ▼

JSON Response

       │

       ▼

setState()

       │

       ▼

Re-render

       │

       ▼

Updated UI
```

---

# Basic Example (fetch API)

```jsx
import {

useEffect,

useState

} from "react";

function Users(){

const[

users,

setUsers

]=useState([]);

useEffect(()=>{

async function fetchUsers(){

const response=

await fetch(

"https://jsonplaceholder.typicode.com/users"

);

const data=

await response.json();

setUsers(data);

}

fetchUsers();

},[]);

return(

<ul>

{

users.map(user=>

<li

key={user.id}

>

{user.name}

</li>

)

}

</ul>

);

}
```

---

# Internal Working

```text
Component Mount

↓

useEffect()

↓

HTTP Request

↓

Server

↓

JSON Response

↓

setUsers()

↓

React Scheduler

↓

Render

↓

Updated UI
```

---

# Using Async/Await

```jsx
async function fetchData(){

const response=

await fetch(url);

const data=

await response.json();

setData(data);

}
```

Cleaner than promise chaining.

---

# Using Axios

Install

```bash
npm install axios
```

Example

```jsx
import axios from "axios";

useEffect(()=>{

async function load(){

const response=

await axios.get(

"https://jsonplaceholder.typicode.com/users"

);

setUsers(

response.data

);

}

load();

},[]);
```

---

# fetch vs Axios

| fetch | Axios |
|--------|--------|
| Built into browser | External library |
| Manual JSON parsing | Automatic JSON parsing |
| Manual error handling | Better error handling |
| Smaller bundle | More features |

---

# Loading State

```jsx
const[

loading,

setLoading

]=useState(true);

const[

users,

setUsers

]=useState([]);
```

```jsx
useEffect(()=>{

async function load(){

const response=

await fetch(url);

const data=

await response.json();

setUsers(data);

setLoading(false);

}

load();

},[]);
```

UI

```jsx
if(

loading

){

return <Spinner/>;

}
```

---

# Error Handling

```jsx
const[

error,

setError

]=useState(null);

useEffect(()=>{

async function load(){

try{

const response=

await fetch(url);

if(

!response.ok

){

throw new Error(

"API Error"

);

}

const data=

await response.json();

setUsers(data);

}

catch(err){

setError(err.message);

}

}

load();

},[]);
```

Display

```jsx
if(error){

return <p>

{error}

</p>;

}
```

---

# Complete Production Example

```jsx
function Users(){

const[

users,

setUsers

]=useState([]);

const[

loading,

setLoading

]=useState(true);

const[

error,

setError

]=useState("");

useEffect(()=>{

let ignore=false;

async function load(){

try{

const response=

await fetch(url);

if(!response.ok){

throw new Error("Failed");

}

const data=

await response.json();

if(!ignore){

setUsers(data);

}

}

catch(err){

if(!ignore){

setError(err.message);

}

}

finally{

if(!ignore){

setLoading(false);

}

}

}

load();

return()=>{

ignore=true;

};

},[]);

if(loading)

return <Spinner/>;

if(error)

return <Error/>;

return(

<UserList

users={users}

/>

);

}
```

---

# AbortController (Best Practice)

Prevent state updates after unmount.

```jsx
useEffect(()=>{

const controller=

new AbortController();

async function load(){

try{

const response=

await fetch(

url,

{

signal:

controller.signal

}

);

const data=

await response.json();

setUsers(data);

}

catch(error){

if(

error.name!=="AbortError"

){

console.error(error);

}

}

}

load();

return()=>{

controller.abort();

};

},[]);
```

---

# Why Abort Requests?

Imagine

```
User Opens Page

↓

API Call Starts

↓

User Leaves Page

↓

API Returns

↓

Component Already Removed
```

Without cancellation,

React may try to update a component that no longer exists.

---

# Parallel API Calls

```jsx
const[

users,

posts

]=await Promise.all([

fetch(usersUrl),

fetch(postsUrl)

]);
```

Faster than sequential requests.

---

# Sequential Calls

```jsx
const user=

await fetch(userUrl);

const orders=

await fetch(

`orders/${user.id}`

);
```

When one request depends on another.

---

# React Query (TanStack Query) ⭐

Modern enterprise applications rarely fetch data manually.

```jsx
const{

data,

isLoading,

error

}=useQuery({

queryKey:["users"],

queryFn:fetchUsers

});
```

Benefits

- Automatic Caching
- Background Refetch
- Retry
- Pagination
- Optimistic Updates
- Request Deduplication

---

# SWR

```jsx
const{

data,

error

}=useSWR(

"/users",

fetcher

);
```

Popular in Next.js applications.

---

# Internal Working

```text
Component Mount

↓

useEffect()

↓

HTTP Request

↓

Server

↓

Response

↓

State Update

↓

Fiber Scheduler

↓

Render Phase

↓

Virtual DOM

↓

Reconciliation

↓

Commit Phase

↓

Updated UI
```

---

# Common Mistakes

## Calling fetch in Component Body

Wrong

```jsx
function App(){

fetch(url);

}
```

Runs on every render.

---

Correct

```jsx
useEffect(()=>{

fetchData();

},[]);
```

---

## Missing Loading State

Wrong

```jsx
return(

<div>

{users.name}

</div>

);
```

Data may not be available yet.

---

Correct

```jsx
if(

loading

)

return<Spinner/>;
```

---

## Ignoring Errors

Always handle

- Network errors
- HTTP errors
- Timeouts
- Empty responses

---

## Forgetting Cleanup

Cancel long-running requests when the component unmounts using `AbortController` (or library-specific cancellation).

---

# Best Practices

- Fetch data in `useEffect` (or use React Query/SWR).
- Show loading and error states.
- Cancel requests on unmount when appropriate.
- Validate API responses.
- Keep fetching logic in custom hooks or service files for reuse.
- Prefer React Query/TanStack Query for complex applications.

---

# Interview Questions

## Beginner

### What is data fetching?

Requesting data from an external API or server.

---

### Which Hook is commonly used?

```jsx
useEffect()
```

---

### Why use `useState`?

To store the fetched data.

---

### Why use `useEffect`?

To control when the API request runs.

---

# Intermediate

### Difference between fetch and Axios?

| fetch | Axios |
|--------|--------|
| Native API | Third-party library |
| Manual JSON parsing | Automatic JSON parsing |
| Simpler | More features |

---

### Why do we show loading state?

Because the request is asynchronous and the UI should provide feedback while waiting.

---

### How do you handle errors?

Using `try...catch` and checking `response.ok`.

---

# Senior-Level Interview Questions

### Why is React Query preferred in enterprise applications?

It provides:

- Caching
- Automatic retries
- Background refetching
- Pagination
- Optimistic updates
- Reduced boilerplate
- Better developer experience

---

### How do you prevent race conditions?

- Cancel previous requests with `AbortController`.
- Ignore outdated responses.
- Use React Query, which manages request lifecycles.

---

### Why shouldn't you fetch data inside the component body?

Because the component function runs on every render, causing repeated network requests.

---

### How do you optimize multiple API calls?

- Use `Promise.all()` for independent requests.
- Cache responses.
- Use pagination and lazy loading.
- Memoize derived data when appropriate.

---

# Scenario-Based Questions

## Scenario 1

An interviewer asks:

**"Why is my API called repeatedly?"**

**Answer**

The request is inside the component body or the effect dependencies are incorrect.

---

## Scenario 2

The user navigates away before the API finishes.

What should you do?

**Answer**

Abort the request using `AbortController` or use a library that handles cancellation.

---

## Scenario 3

Your application fetches users, posts, and comments simultaneously.

How would you optimize it?

**Answer**

Use:

```jsx
Promise.all([
fetchUsers(),
fetchPosts(),
fetchComments()
]);
```

---

## Scenario 4

A dashboard refetches the same data on every page visit.

How would you improve it?

**Answer**

Use TanStack Query (React Query) or SWR to cache responses and automatically manage stale data.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Hook | `useEffect()` |
| State | `useState()` |
| API | `fetch()` / Axios |
| Loading | `loading` state |
| Error | `try...catch` |
| Cleanup | `AbortController` |
| Enterprise | TanStack Query |
| Multiple APIs | `Promise.all()` |

---

# Excalidraw Diagram

```text
                  Component Mount

                        │

                   useEffect()

                        │

                  HTTP Request

                        │

          ┌─────────────┼─────────────┐

          │                           │

      Success                     Failure

          │                           │

   Parse JSON                  Error Handling

          │                           │

          └─────────────┬─────────────┘

                        │

                  Update State

                        │

                 React Scheduler

                        │

                  Render Phase

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