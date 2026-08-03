---
title: useState Hook
tags:
  - react
  - hooks
  - usestate
  - state
  - interview
  - rendering
created: 2026-08-02
---

# What is useState?

> [!INFO]
> `useState()` is a **React Hook** that allows a **Function Component** to store, update, and manage state.

> [!IMPORTANT]
> Before React 16.8, only **Class Components** could manage state using `this.state`.

> After Hooks were introduced, **Function Components** became capable of managing state using `useState()`.

---

# Beginner Explanation

Imagine a whiteboard.

```
Current Value

↓

Erase

↓

Write New Value

↓

Everyone sees updated value
```

React state works the same way.

```
State

↓

Update State

↓

React Re-renders

↓

Updated UI
```

---

# What is State?

State is **data owned and managed by a component** that can change over time.

Examples

- Counter
- User Input
- Login Status
- Theme
- Shopping Cart
- API Response

---

# Before Hooks

Only Class Components

```jsx
class Counter extends React.Component{

    state={

        count:0

    };

}
```

---

# After Hooks

Function Components

```jsx
const [

count,

setCount

]=useState(0);
```

Cleaner and easier.

---

# Syntax

```jsx
const [

state,

setState

]=useState(initialValue);
```

Example

```jsx
const [

count,

setCount

]=useState(0);
```

Where

```
count

↓

Current State
```

```
setCount()

↓

Updates State
```

```
0

↓

Initial Value
```

---

# Visual Flow

```text
useState(0)

        │

        ▼

count = 0

        │

Button Click

        │

setCount(1)

        │

React Re-render

        │

count = 1
```

---

# Basic Counter Example

```jsx
import {

useState

} from "react";

function Counter(){

const [

count,

setCount

]=useState(0);

return(

<>

<h2>

{count}

</h2>

<button

onClick={()=>

setCount(

count+1

)

}

>

Increment

</button>

</>

);

}
```

Output

```
0

↓

1

↓

2

↓

3
```

---

# How useState Works Internally

```text
Component Render

↓

useState(0)

↓

State Stored Inside React Fiber

↓

User Click

↓

setState()

↓

Scheduler

↓

Render Phase

↓

Virtual DOM

↓

Reconciliation

↓

Commit Phase

↓

Browser Updates UI
```

---

# Destructuring

```jsx
const [

count,

setCount

]=useState(0);
```

Equivalent to

```jsx
const state=

useState(0);

const count=

state[0];

const setCount=

state[1];
```

---

# Updating State

Wrong

```jsx
count++;
```

React doesn't know state changed.

---

Correct

```jsx
setCount(

count+1

);
```

---

# Functional Update (Recommended)

Wrong

```jsx
setCount(

count+1

);

setCount(

count+1

);
```

Result

```
+1
```

---

Correct

```jsx
setCount(

prev=>prev+1

);

setCount(

prev=>prev+1

);
```

Result

```
+2
```

---

# Why Functional Updates?

React batches state updates.

Always use the previous state when the next value depends on it.

---

# State is Asynchronous

```jsx
setCount(

count+1

);

console.log(count);
```

Output

```
Old Value
```

Because React schedules the update and applies it on the next render.

---

# Updating Object State

Wrong

```jsx
const [

user,

setUser

]=useState({

name:"John",

age:25

});

user.age=30;
```

Never mutate state.

---

Correct

```jsx
setUser({

...user,

age:30

});
```

---

# Updating Nested Object

```jsx
const [

user,

setUser

]=useState({

name:"John",

address:{

city:"Mumbai"

}

});
```

```jsx
setUser({

...user,

address:{

...user.address,

city:"Pune"

}

});
```

---

# Updating Array State

```jsx
const [

users,

setUsers

]=useState([]);
```

Add Item

```jsx
setUsers([

...users,

newUser

]);
```

Remove Item

```jsx
setUsers(

users.filter(

user=>

user.id!==id

)

);
```

Update Item

```jsx
setUsers(

users.map(

user=>

user.id===id

?

{

...user,

name:"Alex"

}

:

user

)

);
```

---

# Multiple States

```jsx
const [

name,

setName

]=useState("");

const [

age,

setAge

]=useState(20);

const [

city,

setCity

]=useState("");
```

Recommended for unrelated values.

---

# Single Object State

```jsx
const [

form,

setForm

]=useState({

name:"",

email:"",

password:""

});
```

Recommended for related form fields.

---

# Lazy Initialization

```jsx
const [

users,

setUsers

]=useState(

()=>{

return

loadUsers();

}

);
```

The initialization function runs only once during the initial render.

Useful for expensive initial computations.

---

# Real-World Examples

## Counter

```jsx
const [

count,

setCount

]=useState(0);
```

---

## Login

```jsx
const [

loggedIn,

setLoggedIn

]=useState(false);
```

---

## Theme

```jsx
const [

theme,

setTheme

]=useState("light");
```

---

## Shopping Cart

```jsx
const [

cart,

setCart

]=useState([]);
```

---

## API Data

```jsx
const [

users,

setUsers

]=useState([]);
```

---

# Internal Working

```text
useState()

↓

Store State

↓

User Interaction

↓

setState()

↓

React Scheduler

↓

Fiber Tree

↓

Render Phase

↓

Virtual DOM

↓

Diffing

↓

Commit Phase

↓

Updated UI
```

---

# Common Mistakes

## Mutating State

Wrong

```jsx
user.name="John";
```

Correct

```jsx
setUser({

...user,

name:"John"

});
```

---

## Updating Based on Current State

Wrong

```jsx
setCount(

count+1

);
```

Use

```jsx
setCount(

prev=>prev+1

);
```

when the next state depends on the previous state.

---

## Calling setState in Render

Wrong

```jsx
function App(){

setCount(5);

}
```

Causes an infinite render loop.

---

## Forgetting Immutable Updates

Wrong

```jsx
array.push(item);
```

Correct

```jsx
setArray([

...array,

item

]);
```

---

# Best Practices

- Treat state as immutable.
- Use functional updates when the next state depends on the previous one.
- Split unrelated state into separate variables.
- Group related fields (like forms) into objects.
- Avoid storing derived values in state if they can be computed during rendering.
- Keep state as minimal as possible.

---

# Interview Questions

## Beginner

### What is useState?

A Hook that lets Function Components store and update state.

---

### What does useState return?

An array containing:

- Current state
- State updater function

---

### Why can't we modify state directly?

Because React won't know the state changed and won't schedule a re-render.

---

### Does setState update immediately?

No.

State updates are scheduled and processed before the next render.

---

# Intermediate

### Why use functional updates?

To safely calculate the next state from the previous state, especially when multiple updates may be batched.

---

### Can useState store objects and arrays?

Yes.

It can store any JavaScript value.

---

### How do you update an object?

Using the spread operator.

```jsx
setUser({

...user,

age:30

});
```

---

### Difference between multiple useState calls and one object?

Use multiple `useState` calls for unrelated values.

Use a single object for logically related fields (e.g., form data).

---

# Senior-Level Interview Questions

### Where does React store state?

React stores Hook state in the component's **Fiber node**. Each render walks through the hooks in the same order, allowing React to associate each `useState` call with its stored state.

---

### Why must Hooks always be called in the same order?

React identifies Hook state by **call order**, not by variable name.

Changing the order (e.g., calling Hooks conditionally) breaks this mapping and causes incorrect behavior.

---

### Why are state updates asynchronous?

React schedules updates to optimize rendering, batch multiple updates, and avoid unnecessary work before committing changes to the DOM.

---

### Difference between useState and useReducer?

| useState | useReducer |
|----------|------------|
| Simple state | Complex state logic |
| Easy to learn | Better for multiple related actions |
| Direct updates | Reducer-based updates |

---

# Scenario-Based Questions

## Scenario 1

An interviewer asks:

**"Why does this print the old value?"**

```jsx
setCount(

count+1

);

console.log(count);
```

**Answer**

Because state updates are scheduled. The new value is available on the next render.

---

## Scenario 2

Clicking a button twice only increases the counter by 1.

```jsx
setCount(

count+1

);

setCount(

count+1

);
```

**Answer**

Both updates use the same stale value.

Use:

```jsx
setCount(

prev=>prev+1

);

setCount(

prev=>prev+1

);
```

---

## Scenario 3

A developer writes:

```jsx
user.name="Alex";
```

Why doesn't the UI update?

**Answer**

The object was mutated directly.

React requires immutable updates using the setter function.

---

## Scenario 4

You have a registration form with:

- Name
- Email
- Password

Should you use three `useState` hooks or one object?

**Answer**

Both approaches work.

A single object is often more convenient because the fields belong to the same form.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Hook | `useState()` |
| Purpose | Manage local state |
| Returns | `[state, setState]` |
| Updates | `setState()` |
| Direct Mutation | ❌ Never |
| Functional Update | `prev => prev + 1` |
| Objects | Update with spread operator |
| Arrays | Use immutable methods (`map`, `filter`, spread) |
| Async | State updates are scheduled |

---

# Excalidraw Diagram

```text
                    useState()

                         │

               Initial State Created

                         │

                  Current State Value

                         │

                  User Interaction

                         │

                    setState()

                         │

                  React Scheduler

                         │

                   Fiber Updates

                         │

                  Render Phase

                         │

                  Virtual DOM

                         │

                 Reconciliation

                         │

                  Commit Phase

                         │

                     Updated UI
```

---

# Related Notes

- 