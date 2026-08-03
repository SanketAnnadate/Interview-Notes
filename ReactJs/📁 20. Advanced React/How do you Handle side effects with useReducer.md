---
title: Handling Side Effects with useReducer
tags:
  - react
  - hooks
  - usereducer
  - useeffect
  - async
  - interview
created: 2026-08-03
---

# How do you handle side effects with useReducer?

> [!INFO]
> **useReducer itself cannot perform side effects.**
>
> Side effects such as:
>
> - API Calls
> - Database Requests
> - Timers
> - WebSockets
> - Local Storage
> - DOM Manipulation
>
> should be handled using **useEffect()** or event handlers.
>
> The reducer should only calculate and return the next state.

---

# Beginner Explanation

Imagine a restaurant.

```
Customer

↓

Waiter

↓

Chef

↓

Food
```

The **Chef (Reducer)**

only cooks food.

The chef does **NOT**

- Take orders
- Deliver food
- Collect payment

Those are handled by the waiter.

React works the same way.

```
useEffect()

↓

API Call

↓

dispatch()

↓

Reducer

↓

Update State

↓

UI
```

---

# Why Can't Reducers Perform Side Effects?

Reducers must always be **Pure Functions**.

A pure function:

- Returns the same output for the same input.
- Has no side effects.
- Doesn't modify external data.

Wrong

```jsx
function reducer(state, action) {

    fetch("/users"); // ❌

    return state;
}
```

Correct

```jsx
useEffect(() => {

    fetch("/users")
        .then(res => res.json())
        .then(data => {

            dispatch({

                type: "SUCCESS",

                payload: data

            });

        });

}, []);
```

---

# How useReducer + useEffect Works

```text
Component Mounts

↓

useEffect()

↓

API Call

↓

dispatch()

↓

Reducer

↓

New State

↓

React Re-render

↓

Updated UI
```

---

# Example (Based on the Image)

```jsx
import {

    useReducer,
    useEffect

} from "react";

const initialState = {

    data: null,
    loading: true,
    error: null

};

function reducer(state, action) {

    switch (action.type) {

        case "FETCH_SUCCESS":

            return {

                ...state,
                loading: false,
                data: action.payload

            };

        case "FETCH_ERROR":

            return {

                ...state,
                loading: false,
                error: action.payload

            };

        default:

            return state;

    }

}

export default function DataFetcher() {

    const [state, dispatch] =

        useReducer(

            reducer,
            initialState

        );

    useEffect(() => {

        async function fetchData() {

            try {

                const response =

                    await fetch("https://api.example.com/data");

                const data =

                    await response.json();

                dispatch({

                    type: "FETCH_SUCCESS",

                    payload: data

                });

            } catch (error) {

                dispatch({

                    type: "FETCH_ERROR",

                    payload: error.message

                });

            }

        }

        fetchData();

    }, []);

    if (state.loading)

        return <h2>Loading...</h2>;

    if (state.error)

        return <h2>{state.error}</h2>;

    return <pre>{JSON.stringify(state.data)}</pre>;

}
```

---

# Internal Flow

```text
Page Loads

↓

useEffect()

↓

Fetch API

↓

Server Response

↓

dispatch()

↓

Reducer

↓

New State

↓

React Fiber

↓

Re-render

↓

Display Data
```

---

# Why use useEffect?

Because API calls are **side effects**.

They depend on:

- Internet
- Server
- Database
- External Systems

Reducers should never depend on external systems.

---

# Event Handler Example

Side effects can also happen inside an event.

```jsx
const handleLogin = async () => {

    const response =

        await login();

    dispatch({

        type: "LOGIN_SUCCESS",

        payload: response

    });

};
```

Notice

```
API

↓

dispatch

↓

Reducer
```

The reducer never performs the API call.

---

# Real World Example

## Banking App

User clicks

```
Transfer Money
```

Flow

```text
Click Transfer

↓

Call Banking API

↓

Success

↓

dispatch({

TRANSFER_SUCCESS

})

↓

Reducer

↓

Update Balance

↓

UI Refresh
```

---

## Shopping Cart

```text
Click Buy

↓

API Request

↓

Payment Success

↓

dispatch()

↓

Reducer

↓

Update Cart

↓

Render Success
```

---

# Common Mistakes

## Calling API Inside Reducer

Wrong

```jsx
function reducer(state, action) {

    fetch("/users");

    return state;

}
```

Reducers must remain pure.

---

## Async Reducer

Wrong

```jsx
async function reducer() {

}
```

Reducers should **never** be async.

---

## Using await Inside Reducer

Wrong

```jsx
const data =

await fetch(...);
```

Always use `useEffect` or an event handler.

---

# Best Practices

- Keep reducers pure.
- Perform API calls in `useEffect`.
- Dispatch actions after async work completes.
- Handle loading, success, and error separately.
- Never mutate state.
- Return a new state object.

---

# Interview Questions

## Beginner

### Can useReducer make API calls?

❌ No.

Use `useEffect()` or event handlers.

---

### Why?

Because reducers must be pure functions.

---

### What is a side effect?

Anything outside normal rendering.

Examples:

- API Calls
- Timers
- Local Storage
- WebSocket
- DOM Updates

---

# Intermediate

### Why combine useReducer with useEffect?

`useEffect` performs async work.

`useReducer` manages predictable state transitions.

---

### Typical Async States

```text
Loading

↓

Success

↓

Error
```

Actions

```text
FETCH_START

FETCH_SUCCESS

FETCH_ERROR
```

---

# Senior-Level Interview Questions

### Why should reducers be pure?

Pure reducers make state updates predictable, easier to debug, and compatible with React's rendering model.

---

### Why not perform API calls inside reducers?

Reducers may be executed multiple times during development (e.g., Strict Mode) and should not trigger external side effects.

---

### How do enterprise applications manage async logic?

Common approaches:

- `useEffect + useReducer`
- Redux Toolkit + createAsyncThunk
- TanStack Query (React Query)
- SWR

---

# Scenario-Based Questions

## Scenario 1

You need to fetch user data when the component loads.

How?

**Answer**

Call the API inside `useEffect` and dispatch the result to the reducer.

---

## Scenario 2

Where should loading state be updated?

**Answer**

Inside the reducer.

Example actions:

```
FETCH_START

FETCH_SUCCESS

FETCH_ERROR
```

---

## Scenario 3

Can a reducer call `fetch()`?

**Answer**

No.

Only `useEffect` or event handlers should perform side effects.

---

## Scenario 4

Why separate async logic from reducers?

**Answer**

It keeps reducers pure, predictable, and easy to test while isolating external operations.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Side Effects | API Calls, Timers, Local Storage |
| Hook for Side Effects | `useEffect` |
| Reducer | Pure Function |
| Async Logic | Outside Reducer |
| Update State | `dispatch()` |
| Enterprise Pattern | `useEffect + useReducer` |

---

# Excalidraw Diagram

```text
                  Component Mount

                         │

                         ▼

                   useEffect()

                         │

                  API Request

                         │

                Server Response

                         │

              dispatch(action)

                         │

                     Reducer

                         │

                 New State Object

                         │

                  React Fiber

                         │

                  Re-render UI

                         │

                 Updated Screen


──────────────────────────────────────────────

        Wrong ❌

Reducer

↓

API Call

↓

State Update


──────────────────────────────────────────────

        Correct ✅

useEffect

↓

API Call

↓

dispatch()

↓

Reducer

↓

State Update

↓

UI
```

---

# Related Notes

- [[useReducer]]
- [[useEffect]]
- 