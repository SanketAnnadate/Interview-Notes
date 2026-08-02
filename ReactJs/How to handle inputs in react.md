---
title: Handling Inputs in React
tags:
  - react
  - forms
  - controlled-components
  - uncontrolled-components
  - state
  - interview
created: 2026-08-02
---

# How to Handle Inputs in React?

> [!INFO]
> In React, form inputs are typically handled using **Controlled Components**, where the input value is managed by the component's **state**.

> [!IMPORTANT]
> React recommends **Controlled Components** because they provide a **single source of truth**, making forms predictable, easier to validate, and simpler to debug.

---

# Beginner Explanation

Imagine a teacher asking students to fill out a form.

## Normal HTML

The form keeps all the information.

```
Input Box

↓

Stores Value

↓

Submit
```

React doesn't know what's inside until the form is submitted.

---

## React

```
Input Box

↓

User Types

↓

React State Updates

↓

Input Displays State
```

React always knows the latest value.

---

# Two Ways to Handle Inputs

```text
Input Handling

        │

        ├── Controlled Components ⭐

        │

        └── Uncontrolled Components
```

---

# Controlled Components (Recommended)

The value of the input comes from React State.

```text
User Types

↓

onChange Event

↓

setState()

↓

State Updated

↓

Input Updated
```

---

# Basic Example

```jsx
import { useState } from "react";

function NameForm() {

    const [

        name,

        setName

    ] = useState("");

    function handleChange(event) {

        setName(event.target.value);

    }

    return (

        <>

            <input

                type="text"

                value={name}

                onChange={handleChange}

            />

            <h2>

                {name}

            </h2>

        </>

    );

}

export default NameForm;
```

---

# Internal Flow

```text
User Types

↓

onChange

↓

event.target.value

↓

setState()

↓

React Re-render

↓

Input Value Updated
```

---

# Why Do We Need value?

```jsx
<input

    value={name}

/>
```

The displayed value always comes from React state.

Without `value`, React no longer controls the input.

---

# Why Do We Need onChange?

```jsx
onChange={handleChange}
```

Whenever the user types,

React updates the state.

Without `onChange`

```jsx
<input

    value={name}

/>
```

The input becomes **read-only**.

---

# Event Object

```jsx
function handleChange(event){

    console.log(

        event.target.value

    );

}
```

Suppose user types

```
React
```

Output

```
R

Re

Rea

Reac

React
```

Every keystroke triggers `onChange`.

---

# Multiple Inputs

```jsx
const [

    form,

    setForm

] = useState({

    name: "",

    email: ""

});

function handleChange(event){

    setForm({

        ...form,

        [event.target.name]:

            event.target.value

    });

}
```

```jsx
<input

    name="name"

    value={form.name}

    onChange={handleChange}

/>

<input

    name="email"

    value={form.email}

    onChange={handleChange}

/>
```

---

# Checkbox

```jsx
const [

    checked,

    setChecked

] = useState(false);

<input

    type="checkbox"

    checked={checked}

    onChange={(e)=>

        setChecked(

            e.target.checked

        )

    }

/>
```

---

# Radio Button

```jsx
<input

    type="radio"

    value="Male"

    checked={gender==="Male"}

    onChange={(e)=>

        setGender(

            e.target.value

        )

    }

/>
```

---

# Select Dropdown

```jsx
<select

    value={country}

    onChange={(e)=>

        setCountry(

            e.target.value

        )

    }

>

    <option>

        India

    </option>

    <option>

        USA

    </option>

</select>
```

---

# Textarea

```jsx
<textarea

    value={message}

    onChange={(e)=>

        setMessage(

            e.target.value

        )

    }

/>
```

---

# Form Submission

```jsx
function submit(event){

    event.preventDefault();

    console.log(name);

}
```

```jsx
<form

    onSubmit={submit}

>

    ...

</form>
```

---

# Uncontrolled Components

Instead of state,

React reads the value directly from the DOM.

```jsx
import {

    useRef

} from "react";

function App(){

    const inputRef = useRef();

    function submit(){

        console.log(

            inputRef.current.value

        );

    }

    return(

        <>

            <input

                ref={inputRef}

            />

            <button

                onClick={submit}

            >

                Submit

            </button>

        </>

    );

}
```

---

# Controlled vs Uncontrolled

| Controlled | Uncontrolled |
|------------|--------------|
| Uses State | Uses DOM |
| Predictable | Less Predictable |
| Easy Validation | Manual Validation |
| Easy Debugging | Harder Debugging |
| Recommended | Limited Use Cases |

---

# Real-World Example

## Login Form

```text
Username

↓

State

↓

Validation

↓

Submit
```

---

## Registration Form

```
Name

↓

Email

↓

Password

↓

State

↓

API
```

---

## Search Bar

```jsx
<input

    value={search}

    onChange={(e)=>

        setSearch(

            e.target.value

        )

    }

/>
```

Every key updates the search results.

---

# Internal Working

```text
User Types

↓

Browser Event

↓

React Synthetic Event

↓

onChange

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

Updated Input
```

---

# Common Mistakes

## Forgetting value

Wrong

```jsx
<input

    onChange={handleChange}

/>
```

React doesn't control the input.

---

Correct

```jsx
<input

    value={name}

    onChange={handleChange}

/>
```

---

## Forgetting onChange

Wrong

```jsx
<input

    value={name}

/>
```

The input becomes read-only.

---

## Mutating State

Wrong

```jsx
form.name = "John";
```

Correct

```jsx
setForm({

    ...form,

    name: "John"

});
```

---

# Best Practices

- Use controlled components for most forms.
- Keep form state in React.
- Use `event.preventDefault()` on form submission.
- Use one `handleChange` function for multiple fields.
- Validate input before submitting.
- Use libraries like **React Hook Form** or **Formik** for large forms.

---

# Interview Questions

## Beginner

### What is a Controlled Component?

A form element whose value is controlled by React state.

---

### Why do we use `value`?

To make React the source of truth for the input.

---

### Why do we use `onChange`?

To update React state whenever the user types.

---

### What does `event.target.value` return?

The current value of the input field.

---

# Intermediate

### Difference between Controlled and Uncontrolled Components?

| Controlled | Uncontrolled |
|------------|--------------|
| React State | DOM |
| Predictable | Less Predictable |
| Easy Validation | Manual Validation |

---

### Why does an input become read-only?

Because it has a `value` prop without an `onChange` handler.

---

### Why use a single `handleChange` function?

It simplifies handling multiple form fields and reduces duplicate code.

---

# Senior-Level Interview Questions

### Why are controlled components preferred in enterprise applications?

Because they provide predictable state management, easier validation, better integration with business logic, and improved debugging.

---

### When would you choose an uncontrolled component?

When integrating with third-party libraries, handling simple file inputs, or optimizing very large forms where DOM access is sufficient.

---

### How can you optimize large forms?

- React Hook Form
- Formik
- Debouncing expensive operations
- Splitting forms into smaller components
- Memoization where appropriate

---

# Scenario-Based Questions

## Scenario 1

An interviewer asks:

**"Why is my input read-only?"**

```jsx
<input

    value={name}

/>
```

**Answer**

Because a controlled input requires an `onChange` handler to update its value.

---

## Scenario 2

A form contains 20 input fields.

Would you create 20 `handleChange` functions?

**Answer**

No.

Use a single generic `handleChange` based on the input's `name` attribute.

---

## Scenario 3

How do you access the value of an uncontrolled input?

**Answer**

Using a `ref`.

```jsx
const inputRef = useRef();

console.log(

    inputRef.current.value

);
```

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Controlled Component | State controls input |
| Uncontrolled Component | DOM controls input |
| value | Current state value |
| onChange | Updates state |
| event.target.value | Current input text |
| checked | Checkbox state |
| preventDefault() | Prevent form refresh |
| useRef | Access uncontrolled input |

---

# Excalidraw Diagram

```text
                 User Types

                      │

                onChange Event

                      │

            event.target.value

                      │

                setState()

                      │

               React Scheduler

                      │

               Render Phase

                      │

                Virtual DOM

                      │

               Reconciliation

                      │

               Commit Phase

                      │

              Updated Input Field
```

---

# Related Notes

- 