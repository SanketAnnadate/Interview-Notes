---
title: Passing Additional Arguments to useReducer (Payload)
tags:
  - react
  - hooks
  - usereducer
  - payload
  - dispatch
  - interview
created: 2026-08-02
---

# How do you pass additional arguments to the reducer function?

> [!INFO]
> In **useReducer**, additional data is passed to the reducer through the **action object** using a property called **payload**.

> [!IMPORTANT]
> The reducer function **always accepts only two parameters**:
>
> ```jsx
> reducer(state, action)
> ```
>
> You **cannot** pass extra parameters directly.
>
> Instead, send additional information inside the **action object**.

---

# Beginner Explanation

Imagine you're ordering food.

You don't just say

```
Order
```

You also specify

```
Pizza

Size

Quantity

Address
```

This extra information is called the **payload**.

React works exactly the same way.

Instead of saying

```
Update
```

You say

```
Update

+

New Data
```

---

# Why Payload?

Suppose we have

```jsx
state = {

count:0

}
```

If we dispatch

```jsx
dispatch({

type:"increment"

});
```

React only knows

```
Increment
```

But what if we want

```
Increase by 10

or

Update User

or

Delete Product
```

We need extra information.

That information is called the **payload**.

---

# Action Object Structure

```text
Action

│

├── type

└── payload
```

Example

```jsx
{

type:"UPDATE",

payload:{

value:42

}

}
```

---

# Basic Example

Reducer

```jsx
function reducer(

state,

action

){

switch(action.type){

case "update":

return{

...state,

count:action.payload

};

default:

return state;

}

}
```

Dispatch

```jsx
dispatch({

type:"update",

payload:10

});
```

New State

```jsx
{

count:10

}
```

---

# Passing Object as Payload

Reducer

```jsx
function reducer(

state,

action

){

switch(action.type){

case "UPDATE_USER":

return{

...state,

user:action.payload

};

default:

return state;

}

}
```

Dispatch

```jsx
dispatch({

type:"UPDATE_USER",

payload:{

id:1,

name:"John",

city:"London"

}

});
```

---

# Passing Multiple Values

Dispatch

```jsx
dispatch({

type:"UPDATE",

payload:{

name:"John",

age:30,

city:"London"

}

});
```

Reducer

```jsx
case "UPDATE":

return{

...state,

...action.payload

};
```

---

# Example from Image

Reducer

```jsx
function reducer(state, action) {

    switch (action.type) {

        case "update":

            return {

                ...state,

                value: action.payload.value

            };

        default:

            return state;
    }

}
```

Dispatch

```jsx
dispatch({

type:"update",

payload:{

value:42

}

});
```

New State

```jsx
{

value:42

}
```

---

# Data Flow

```text
Button Click

↓

dispatch()

↓

Action Object

↓

Reducer

↓

action.payload

↓

New State

↓

React Re-render

↓

Updated UI
```

---

# Complete Example

```jsx
import {

useReducer

} from "react";

const initialState={

count:0

};

function reducer(state,action){

switch(action.type){

case "increment":

return{

count:

state.count+

action.payload

};

default:

return state;

}

}

export default function Counter(){

const[

state,

dispatch

]=

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

type:"increment",

payload:5

})}

>

+5

</button>

</>

);

}
```

Output

```
0

↓

5

↓

10

↓

15
```

---

# Real World Example

Shopping Cart

Dispatch

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

cart:[

...state.cart,

action.payload

]

};
```

---

# Login Example

Dispatch

```jsx
dispatch({

type:"LOGIN_SUCCESS",

payload:user

});
```

Reducer

```jsx
case "LOGIN_SUCCESS":

return{

...state,

loading:false,

user:action.payload

};
```

---

# Delete Example

Dispatch

```jsx
dispatch({

type:"DELETE_USER",

payload:5

});
```

Reducer

```jsx
case "DELETE_USER":

return{

...state,

users:

state.users.filter(

user=>user.id!==action.payload

)

};
```

---

# Internal Working

```text
dispatch({

type:"UPDATE",

payload:{value:42}

})

        │

        ▼

Reducer(state,action)

        │

Reads action.payload

        │

Creates New State

        │

React Re-render

        │

Updated UI
```

---

# Why use Payload?

Without Payload

```jsx
dispatch({

type:"update"

});
```

Reducer doesn't know

```
What to update?
```

---

With Payload

```jsx
dispatch({

type:"update",

payload:{

value:42

}

});
```

Reducer has all required information.

---

# Common Mistakes

## Passing Multiple Arguments

Wrong

```jsx
dispatch(

"update",

10

);
```

---

Correct

```jsx
dispatch({

type:"update",

payload:10

});
```

---

## Mutating State

Wrong

```jsx
state.count=

action.payload;
```

---

Correct

```jsx
return{

...state,

count:

action.payload

};
```

---

## Forgetting Payload

Wrong

```jsx
dispatch({

type:"update"

});
```

Reducer expects

```jsx
action.payload
```

Result

```
undefined
```

---

# Best Practices

- Always use an **action object**.
- Keep action names descriptive.
- Store additional data inside `payload`.
- Never mutate state.
- Return a new state object.

---

# Interview Questions

## Beginner

### Can reducer accept three parameters?

❌ No.

Only

```jsx
(state, action)
```

---

### How do you send additional data?

Using

```jsx
payload
```

---

### What is payload?

Extra information passed with an action.

---

# Intermediate

### Why use payload?

Because different actions require different data.

Examples

```
User

Product

ID

Count

Theme
```

---

### Can payload be an object?

Yes.

It can contain

```jsx
{

id,

name,

price,

quantity

}
```

---

# Senior-Level Interview Questions

### Why is payload preferred over additional reducer parameters?

Because React's `dispatch` API always sends a single action object. Packaging data into `payload` keeps actions consistent, extensible, and easier to debug.

---

### Is payload mandatory?

No.

Simple actions like

```jsx
dispatch({

type:"RESET"

});
```

don't require payload.

---

### Why is action usually structured as `{ type, payload }`?

It's a common convention used across React, Redux, and Redux Toolkit, making code predictable and easy to understand.

---

# Scenario-Based Questions

## Scenario 1

You want to increase the counter by **5** instead of **1**.

How?

**Answer**

```jsx
dispatch({

type:"increment",

payload:5

});
```

---

## Scenario 2

You need to update an entire user object.

How?

**Answer**

Pass the user object in `payload`.

---

## Scenario 3

Your reducer receives

```jsx
action.payload

↓

undefined
```

Why?

**Answer**

The dispatched action didn't include a `payload`.

---

## Scenario 4

Can payload contain multiple values?

**Answer**

Yes.

```jsx
payload:{

id:1,

name:"John",

role:"Admin"

}
```

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Extra Data | Payload |
| Sent Through | dispatch() |
| Reducer Reads | action.payload |
| Action Structure | `{ type, payload }` |
| Can Payload Be Object? | Yes |
| Reducer Parameters | `state`, `action` |

---

# Excalidraw Diagram

```text
            Button Click

                  │

                  ▼

dispatch({

type:"UPDATE",

payload:{value:42}

})

                  │

                  ▼

          Action Object

                  │

     ┌────────────┴────────────┐

     │                         │

   type                    payload

 UPDATE                  {value:42}

     │                         │

     └────────────┬────────────┘

                  ▼

             Reducer

                  │

     action.payload.value

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

- [[useReducer]]
```