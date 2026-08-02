---
title: Managing Loading State in React
tags:
  - react
  - loading
  - api
  - useeffect
  - usestate
  - interview
created: 2026-08-02
---

# How to Manage Loading State in React?

> [!INFO]
> A **Loading State** indicates that an operation (such as fetching data from an API) is currently in progress. It provides visual feedback to the user while waiting for the operation to complete.

> [!IMPORTANT]
> Every API call should manage **three states**:
>
> - Loading
> - Success
> - Error
>
> This pattern is commonly used in enterprise React applications.

---

# Beginner Explanation

Imagine ordering food online.

```
Order Placed

↓

Restaurant Preparing

↓

Food Delivered
```

While the food is being prepared, the app displays:

```
Preparing your order...
```

Similarly, React shows a loading message or spinner while waiting for an API response.

```
User Opens Page

↓

Loading...

↓

API Response

↓

Display Data
```

---

# Why Do We Need Loading State?

Without Loading State

```
User Opens Page

↓

Blank Screen

↓

Nothing Happens

↓

Confusing
```

---

With Loading State

```
User Opens Page

↓

Loading Spinner

↓

User Knows

↓

Data Arrives

↓

UI Updates
```

---

# Loading State Flow

```text
Component Mount

      │

      ▼

API Request Starts

      │

      ▼

loading = true

      │

      ▼

Show Spinner

      │

      ▼

Response Received

      │

      ▼

loading = false

      │

      ▼

Display Data
```

---

# Basic Example

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

const[

loading,

setLoading

]=useState(true);

useEffect(()=>{

async function fetchUsers(){

const response=

await fetch(

"https://jsonplaceholder.typicode.com/users"

);

const data=

await response.json();

setUsers(data);

setLoading(false);

}

fetchUsers();

},[]);

if(

loading

){

return <h2>

Loading...

</h2>;

}

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
Page Loads

↓

loading = true

↓

API Request

↓

Waiting...

↓

Response Received

↓

setUsers()

↓

loading = false

↓

React Re-render

↓

Show Data
```

---

# Better Production Example

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

async function load(){

try{

setLoading(true);

const response=

await fetch(url);

if(

!response.ok

){

throw new Error(

"Failed"

);

}

const data=

await response.json();

setUsers(data);

}

catch(err){

setError(

err.message

);

}

finally{

setLoading(false);

}

}

load();

},[]);

if(

loading

)

return <Spinner/>;

if(

error

)

return<Error/>;

return<UserList

users={users}

/>;

}
```

---

# Why Use finally?

Without finally

```
Success

↓

Loading Ends
```

But

```
Error

↓

Loading Never Stops
```

---

With finally

```
Success

↓

Loading Ends

OR

Error

↓

Loading Ends
```

Always executed.

---

# Loading UI Options

## Text

```jsx
<h2>

Loading...

</h2>
```

---

## Spinner

```jsx
<Spinner/>
```

---

## Skeleton Loader ⭐

```
██████████████

██████████████

██████████████
```

Looks like the future content.

Used by

- YouTube
- Facebook
- LinkedIn
- Amazon

---

## Progress Bar

```
███████------

65%
```

Useful for uploads.

---

# Multiple API Calls

```jsx
const[

loading,

setLoading

]=useState(true);

useEffect(()=>{

async function load(){

await Promise.all([

fetchUsers(),

fetchPosts(),

fetchComments()

]);

setLoading(false);

}

load();

},[]);
```

Loading ends only after all requests finish.

---

# Separate Loading States

```jsx
const[

usersLoading,

setUsersLoading

]=useState(false);

const[

postsLoading,

setPostsLoading

]=useState(false);
```

Useful when different sections load independently.

---

# Global Loading State

Large applications often use

```
Redux

or

Context API
```

```
Loading

↓

Entire Application

↓

Loader Overlay
```

---

# React Query (TanStack Query)

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

No need to manage

```jsx
loading

error

data
```

manually.

---

# SWR

```jsx
const{

data,

error,

isLoading

}=useSWR(

"/users",

fetcher

);
```

---

# Internal Working

```text
Component Mount

↓

loading = true

↓

HTTP Request

↓

Server

↓

JSON Response

↓

setData()

↓

loading = false

↓

Render

↓

Updated UI
```

---

# Common Mistakes

## Forgetting Initial Loading State

Wrong

```jsx
const[

loading,

setLoading

]=useState(false);
```

The UI may briefly display incorrect content before the request starts.

---

Correct

```jsx
const[

loading,

setLoading

]=useState(true);
```

---

## Not Using finally

Wrong

```jsx
try{

...

setLoading(false);

}
```

If an error occurs,

loading never becomes false.

---

Correct

```jsx
finally{

setLoading(false);

}
```

---

## Showing Data Before Loading Ends

Wrong

```jsx
return(

<div>

{

users.name

}

</div>

);
```

---

Correct

```jsx
if(

loading

){

return<Spinner/>;

}
```

---

# Best Practices

- Always show a loading indicator during async work.
- Handle loading, success, and error separately.
- Use `finally` to reset loading.
- Prefer skeleton loaders over blank screens for better UX.
- Use React Query or SWR in large applications.

---

# Interview Questions

## Beginner

### What is Loading State?

A UI state indicating that data is currently being fetched or processed.

---

### Why do we use loading state?

To improve user experience by showing progress while waiting.

---

### Which Hook stores loading?

```jsx
useState()
```

---

### Where is loading updated?

Usually inside `useEffect()` during API calls.

---

# Intermediate

### Why should loading be initialized to true?

Because the request starts when the component mounts, and the UI should immediately indicate that work is in progress.

---

### Why use finally?

To ensure loading is reset whether the request succeeds or fails.

---

### How do you show a loader?

```jsx
if(

loading

)

return<Spinner/>;
```

---

# Senior-Level Interview Questions

### How would you manage loading in a large enterprise application?

- TanStack Query (React Query)
- SWR
- Redux
- Context API
- Feature-specific loading states

---

### Difference between global loading and local loading?

| Local | Global |
|--------|---------|
| Single Component | Entire App |
| Faster | Centralized |
| useState | Redux / Context |

---

### Why do skeleton loaders provide a better UX?

They preserve layout, reduce perceived waiting time, and avoid sudden layout shifts when content loads.

---

### How would you handle multiple parallel requests?

Use `Promise.all()` and end loading after all requests complete.

---

# Scenario-Based Questions

## Scenario 1

The spinner never disappears after an API error.

Why?

**Answer**

`setLoading(false)` isn't executed in the error path.

Use a `finally` block.

---

## Scenario 2

An application loads Users, Posts, and Comments together.

How should loading be handled?

**Answer**

Use `Promise.all()` or separate loading states depending on whether the sections are independent.

---

## Scenario 3

The entire application becomes blocked by one loader.

How would you improve it?

**Answer**

Use feature-specific loading indicators so independent sections can render as soon as their data is available.

---

## Scenario 4

Why is React Query preferred?

**Answer**

It automatically manages:

- Loading
- Error
- Caching
- Refetching
- Retry logic
- Background updates

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Loading State | Indicates ongoing work |
| Hook | `useState()` |
| API | `useEffect()` |
| Loading Starts | Before request |
| Loading Ends | `finally` |
| UI | Spinner / Skeleton |
| Enterprise | React Query |

---

# Excalidraw Diagram

```text
                 Component Mount

                        │

                 loading = true

                        │

                  API Request

                        │

              ┌─────────┴─────────┐

              │                   │

          Success             Failure

              │                   │

        setData()           setError()

              │                   │

              └─────────┬─────────┘

                        │

              finally()

                        │

            loading = false

                        │

                React Re-render

                        │

        ┌───────────────┴───────────────┐

        │                               │

    Show Data                     Show Error
```

---

# Related Notes

- 