---
title: Handling Buttons and Click Events in React
tags:
  - react
  - events
  - onclick
  - state
  - interview
  - javascript
created: 2026-08-02
---

# How to Handle Buttons in React?

> [!INFO]
> In React, buttons are handled using **Event Handling**.
>
> The most common event is **onClick**, which executes a JavaScript function when a user clicks a button.

> [!IMPORTANT]
> React uses **Synthetic Events**, a cross-browser wrapper around native browser events, providing a consistent API.

---

# Beginner Explanation

Imagine a TV remote.

```
Button Press

↓

TV Receives Signal

↓

TV Performs Action
```

Similarly,

```
Button Click

↓

React Calls Function

↓

UI Updates
```

The button **does not update the UI itself**.

It calls a function, and that function updates the state.

---

# Basic Flow

```text
User Clicks Button

        │

        ▼

onClick Event

        │

        ▼

JavaScript Function

        │

        ▼

State Update

        │

        ▼

React Re-render

        │

        ▼

Updated UI
```

---

# Basic Example

```jsx
function App() {

    function handleClick() {

        alert("Button Clicked");

    }

    return (

        <button

            onClick={handleClick}

        >

            Click Me

        </button>

    );

}
```

---

# Using Arrow Function

```jsx
function App() {

    return (

        <button

            onClick={() => {

                alert("Hello");

            }}

        >

            Click

        </button>

    );

}
```

---

# Updating State

```jsx
import { useState } from "react";

function Counter() {

    const [

        count,

        setCount

    ] = useState(0);

    function increment() {

        setCount(

            prev => prev + 1

        );

    }

    return (

        <>

            <h2>{count}</h2>

            <button

                onClick={increment}

            >

                Increment

            </button>

        </>

    );

}
```

---

# Passing Parameters

Wrong

```jsx
<button

    onClick={deleteUser(id)}

>

Delete

</button>
```

The function executes immediately during rendering.

---

Correct

```jsx
<button

    onClick={() => deleteUser(id)}

>

Delete

</button>
```

The function runs only when the button is clicked.

---

# Using Event Object

```jsx
function App() {

    function handleClick(event) {

        console.log(event);

        console.log(event.target);

    }

    return (

        <button

            onClick={handleClick}

        >

            Click

        </button>

    );

}
```

---

# Prevent Default

```jsx
function App() {

    function submit(event) {

        event.preventDefault();

        console.log("Form Submitted");

    }

    return (

        <form

            onSubmit={submit}

        >

            <button>

                Submit

            </button>

        </form>

    );

}
```

---

# Stop Event Bubbling

```jsx
function Button() {

    function click(event) {

        event.stopPropagation();

    }

    return (

        <button

            onClick={click}

        >

            Click

        </button>

    );

}
```

---

# Disable Button

```jsx
<button

    disabled={loading}

>

    Save

</button>
```

---

# Conditional Button

```jsx
<button

    disabled={!isValid}

>

    Register

</button>
```

---

# Multiple Buttons

```jsx
function App() {

    return (

        <>

            <button

                onClick={save}

            >

                Save

            </button>

            <button

                onClick={deleteItem}

            >

                Delete

            </button>

            <button

                onClick={cancel}

            >

                Cancel

            </button>

        </>

    );

}
```

---

# Class Component Example

```jsx
import React, {

    Component

} from "react";

class Counter extends Component {

    state = {

        count: 0

    };

    increment = () => {

        this.setState(

            prev => ({

                count:

                    prev.count + 1

            })

        );

    };

    render() {

        return (

            <>

                <h2>

                    {this.state.count}

                </h2>

                <button

                    onClick={this.increment}

                >

                    Increment

                </button>

            </>

        );

    }

}
```

---

# Functional Component (Recommended)

```jsx
import {

    useState

} from "react";

function Counter() {

    const [

        count,

        setCount

    ] = useState(0);

    return (

        <>

            <h2>{count}</h2>

            <button

                onClick={() =>

                    setCount(

                        prev => prev + 1

                    )

                }

            >

                Increment

            </button>

        </>

    );

}
```

---

# Real-World Example

## Shopping Cart

```jsx
<button

    onClick={addToCart}

>

Add To Cart

</button>
```

Click

↓

API

↓

State Updated

↓

Cart Count Updated

---

## Login

```jsx
<button

    onClick={login}

>

Login

</button>
```

Click

↓

Authentication

↓

Dashboard

---

## Delete User

```jsx
<button

    onClick={() =>

        deleteUser(id)

    }

>

Delete

</button>
```

---

# Internal Working

```text
User Click

        │

        ▼

Synthetic Event

        │

        ▼

onClick Handler

        │

        ▼

JavaScript Function

        │

        ▼

State Update

        │

        ▼

Virtual DOM

        │

        ▼

Reconciliation

        │

        ▼

Updated UI
```

---

# Common Mistakes

## Calling Function Immediately

Wrong

```jsx
<button

    onClick={save()}

>

Save

</button>
```

Runs during rendering.

---

Correct

```jsx
<button

    onClick={save}

>

Save

</button>
```

or

```jsx
<button

    onClick={() => save()}

>

Save

</button>
```

---

## Mutating State

Wrong

```jsx
count++;
```

Correct

```jsx
setCount(

    prev => prev + 1

);
```

---

## Not Using Functional Update

Wrong

```jsx
setCount(

    count + 1

);

setCount(

    count + 1

);
```

Result

```
+1
```

Correct

```jsx
setCount(

    prev => prev + 1

);

setCount(

    prev => prev + 1

);
```

Result

```
+2
```

---

# Best Practices

- Keep event handlers small.
- Use descriptive names (`handleSubmit`, `handleDelete`).
- Use functional updates when the next state depends on the previous state.
- Avoid inline arrow functions for heavily rendered lists if performance becomes a concern.
- Prevent default behavior when handling forms.
- Stop propagation only when necessary.

---

# Interview Questions

## Beginner

### How do you handle a button click in React?

Using the `onClick` prop.

```jsx
<button

    onClick={handleClick}

>

Click

</button>
```

---

### What is `onClick`?

A React event handler that executes a function when a button is clicked.

---

### Why don't we write `onClick={handleClick()}`?

Because it executes immediately during rendering instead of waiting for the click event.

---

# Intermediate

### How do you pass parameters to an event handler?

```jsx
<button

    onClick={() =>

        deleteUser(id)

    }

>
```

---

### What is a Synthetic Event?

A React wrapper around native browser events that provides consistent behavior across different browsers.

---

### How do you prevent a form from refreshing the page?

```jsx
event.preventDefault();
```

---

# Senior-Level Interview Questions

### Why is `setCount(prev => prev + 1)` preferred over `setCount(count + 1)`?

Because state updates are asynchronous and may be batched.

Using the functional updater always receives the latest state, preventing stale values.

---

### How does React process a button click internally?

```text
User Click

↓

Browser Event

↓

React Synthetic Event

↓

Event Handler

↓

State Update

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

DOM Updated
```

---

### Can inline arrow functions affect performance?

Yes, because a new function is created on every render.

In most applications the impact is negligible, but for large lists or frequently rendered components, using stable callbacks (or `useCallback` when appropriate) can help reduce unnecessary re-renders.

---

# Scenario-Based Questions

## Scenario 1

A button should delete a user with ID `101`.

How would you implement it?

```jsx
<button

    onClick={() =>

        deleteUser(101)

    }

>

Delete

</button>
```

---

## Scenario 2

A submit button refreshes the page.

How do you stop it?

```jsx
function submit(event) {

    event.preventDefault();

}
```

---

## Scenario 3

An interviewer asks:

**"Why is my button function executing before I click it?"**

**Answer**

Because the function is being invoked during rendering:

```jsx
onClick={save()}
```

Instead, pass the function reference or wrap it in an arrow function.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Click Event | `onClick` |
| Pass Function | `onClick={handleClick}` |
| Pass Parameters | `onClick={() => fn(id)}` |
| Prevent Refresh | `event.preventDefault()` |
| Stop Bubbling | `event.stopPropagation()` |
| Update State | `setState()` / `setCount()` |
| Event Type | Synthetic Event |

---

# Excalidraw Diagram

```text
                User Clicks Button

                        │

                  React onClick

                        │

                Event Handler Runs

                        │

        ┌───────────────┼───────────────┐

        │                               │

   Update State                    Call API

        │                               │

        └───────────────┬───────────────┘

                        │

                  React Scheduler

                        │

                  Virtual DOM

                        │

                 Reconciliation

                        │

                   Updated UI
```

---

# Related Notes

- [[State]]
- [[Updating State]]
- [[useState]]
- [[Synthetic Events]]
- [[Conditional Rendering]]
- [[Components]]
- [[Props]]
- [[React Rendering]]
- [[Reconciliation]]
- [[Performance Optimization]]
- [[useCallback]]