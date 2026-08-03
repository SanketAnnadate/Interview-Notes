---
title: useReducer Hook in React
tags:
  - react
  - hooks
  - usereducer
  - state-management
  - interview
created: 2026-08-02
---

# What is useReducer Hook and When Should You Use It?

> [!INFO]
> **useReducer** is a React Hook used to manage **complex state logic**. It is an alternative to `useState()` when state updates involve multiple values, multiple actions, or complex business logic.

> [!IMPORTANT]
> Think of **useReducer** as a **mini Redux** inside a single component.

---

# Beginner Explanation

Imagine a TV.

The TV doesn't directly increase or decrease the volume.

Instead,

```
You

↓

Press Remote Button

↓

TV Receives Command

↓

TV Updates State
```

React's `useReducer` works exactly like this.

Instead of directly changing state,

we **dispatch an action**.

The reducer decides **how** the state should change.

---

# Why do we need useReducer?

Suppose we are managing

- User
- Loading
- Error
- Products
- Cart

Using `useState`

```jsx
const [user,setUser] = useState(null);
const [loading,setLoading] = useState(false);
const [error,setError] = useState("");
const [cart,setCart] = useState([]);
```

Many state variables become difficult to maintain.

Instead,

```
One State

↓

Reducer

↓

Actions

↓

Updated State
```

---

# When Should You Use useReducer?

Use **useReducer** when:

✅ State is complex

✅ Multiple state values depend on each other

✅ Multiple actions update the same state

✅ Business logic is complex

✅ State transitions should be predictable

---

# When NOT to use useReducer?

For simple state

```jsx
const [count,setCount]=useState(0);
```

useState is simpler.

---

# Syntax

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

---

# Parameters

### reducer

Function that decides how state changes.

---

### initialState

Initial value of state.

---

# Returns

```jsx
state

dispatch
```

---

# How useReducer Works

```text
User Clicks Button

↓

dispatch(action)

↓

Reducer Function

↓

Returns New State

↓

React Re-renders
```

---

# Basic Example

```jsx
import { useReducer } from "react";

const initialState = {

    count:0

};

function reducer(state,action){

    switch(action.type){

        case "increment":

            return{

                count:state.count+1

            };

        case "decrement":

            return{

                count:state.count-1

            };

        default:

            return state;
    }

}

function Counter(){

const [state,dispatch]=

useReducer(

reducer,

initialState

);

return(

<>

<h2>

{state.count}

</h2>

<button

onClick={()=>dispatch({

type:"increment"

})}

>

+

</button>

<button

onClick={()=>dispatch({

type:"decrement"

})}

>

-

</button>

</>

);

}
```

---

# Internal Working

```text
Button Click

↓

dispatch({

type:"increment"

})

↓

Reducer

↓

New State

↓

React Fiber

↓

Re-render

↓

Updated UI
```

---

# Understanding Reducer Function

A reducer receives

```jsx
state

action
```

and returns

```jsx
newState
```

Example

```jsx
function reducer(state,action){

switch(action.type){

case "increment":

return{

count:state.count+1

};

}
}
```

---

# Action Object

Action tells React

"What happened?"

Example

```jsx
dispatch({

type:"increment"

});
```

or

```jsx
dispatch({

type:"delete",

id:10

});
```

Action usually contains

```jsx
type

payload
```

---

# Payload Example

```jsx
dispatch({

type:"ADD_PRODUCT",

payload:product

});
```

Reducer

```jsx
case "ADD_PRODUCT":

return{

...state,

products:[

...state.products,

action.payload

]

};
```

---

# Real World Example

Shopping Cart

State

```jsx
{

cart:[],

loading:false,

error:null

}
```

Actions

```
ADD_ITEM

REMOVE_ITEM

CLEAR_CART

UPDATE_QUANTITY
```

Reducer decides

```
How state changes.
```

---

# Login Example

State

```jsx
{

user:null,

loading:false,

error:null

}
```

Actions

```
LOGIN_START

LOGIN_SUCCESS

LOGIN_FAILED

LOGOUT
```

Very common in enterprise projects.

---

# useReducer vs useState

| useState | useReducer |
|-----------|------------|
| Simple State | Complex State |
| Easy | More Structured |
| Small Components | Large Components |
| Direct Update | Action + Reducer |
| Few Updates | Many Updates |

---

# useReducer vs Redux

| useReducer | Redux Toolkit |
|-------------|---------------|
| Local State | Global State |
| Component Level | Entire Application |
| Built into React | External Library |
| Small Logic | Enterprise Apps |

---

# useReducer + Context API

Very common pattern.

```text
Context Provider

↓

useReducer

↓

Global State

↓

Any Component
```

Many applications use

```
Context

+

useReducer
```

instead of Redux for medium-sized projects.

---

# Internal Architecture

```text
Component

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

Updated UI
```

---

# Common Mistakes

## Mutating State

Wrong

```jsx
state.count++;
```

Always return a new state.

Correct

```jsx
return{

count:state.count+1

};
```

---

## Forgetting Default Case

Wrong

```jsx
switch(action.type){

case "increment":
...
}
```

Always include

```jsx
default:

return state;
```

---

## Multiple useState Instead of Reducer

Wrong

```
15 useState Hooks
```

Better

```
One useReducer
```

---

# Best Practices

- Keep reducer function pure.
- Never mutate state.
- Use meaningful action names.
- Keep actions descriptive.
- Split reducers if they become too large.
- Use Context + useReducer for medium applications.

---

# Interview Questions

## Beginner

### What is useReducer?

A Hook used to manage complex state logic.

---

### What does useReducer return?

```jsx
[state, dispatch]
```

---

### What is dispatch?

A function used to send an action to the reducer.

---

### What is reducer?

A function that receives

```
state

action
```

and returns

```
new state
```

---

# Intermediate

### Difference between useState and useReducer?

| useState | useReducer |
|-----------|------------|
| Simple | Complex |
| Direct Update | Action Based |
| Less Boilerplate | More Structured |

---

### What is Action?

Object describing what happened.

Example

```jsx
{

type:"LOGIN"

}
```

---

### What is Payload?

Extra information sent with action.

```jsx
{

type:"DELETE",

payload:id

}
```

---

# Senior-Level Interview Questions

### Why is useReducer considered predictable?

Because every state change goes through a single reducer function, making updates explicit and easier to trace.

---

### When should you use Context + useReducer?

For medium-sized applications where multiple components need shared state but Redux would be excessive.

Examples:

- Authentication
- Shopping Cart
- Theme
- Notifications

---

### Why must reducers be pure functions?

A reducer should always produce the same output for the same input and should not cause side effects such as API calls or DOM manipulation.

---

### Can you perform API calls inside a reducer?

**No.**

Reducers must remain pure.

API calls should be performed in:

- `useEffect`
- Event handlers
- Async functions

---

# Scenario-Based Questions

## Scenario 1

You have

- loading
- user
- error
- token

Should you use useState?

**Answer**

No.

useReducer is a better choice.

---

## Scenario 2

Your reducer directly modifies state.

```jsx
state.count++;
```

Why is this wrong?

**Answer**

React expects immutable updates.

Always return a new object.

---

## Scenario 3

An interviewer asks:

"When would you prefer Redux over useReducer?"

**Answer**

When the state must be shared across many unrelated components, requires middleware, DevTools, or advanced debugging.

---

## Scenario 4

Can useReducer replace Redux?

**Answer**

For local or medium-sized shared state (often combined with Context), yes.

For large enterprise applications with complex global state, Redux Toolkit is usually a better choice.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Hook | useReducer |
| Used For | Complex State |
| Returns | state, dispatch |
| Reducer Input | state, action |
| Reducer Output | newState |
| Alternative | useState |
| Enterprise | Context + useReducer |

---

# Excalidraw Diagram

```text
               Button Click

                     │

              dispatch(action)

                     │

                Action Object

                     │

            { type:"increment" }

                     │

                  Reducer

                     │

      state + action → newState

                     │

               React Fiber

                     │

               Re-render UI

                     │

             Updated Component


──────────────────────────────────────

           useReducer Flow

User Action

      │

      ▼

dispatch()

      │

      ▼

Reducer

      │

      ▼

New State

      │

      ▼

React Re-render

      │

      ▼

Updated UI
```

---

# Related Notes

- [[Hooks]]
- 