---
title: React Forms
tags:
  - react
  - forms
  - controlled-components
  - uncontrolled-components
  - interview
  - hooks
created: 2026-08-03
---

# How to Manage Forms in React?

> [!INFO]
> Forms in React are managed by controlling the values of form elements through React state. This allows React to become the **Single Source of Truth** for form data.

---

# Beginner Explanation

A form is simply a collection of inputs.

Example

- Username
- Password
- Email
- Phone Number

In normal HTML

```
User types

↓

Browser stores value
```

In React

```
User types

↓

React State stores value

↓

Input displays state
```

React always knows the latest value.

---

# Why React Manages Forms?

Managing forms using React provides

- Real-time validation
- Dynamic UI updates
- Better control
- Easy API integration
- Predictable state management

---

# Two Ways to Handle Forms

```
Forms

├── Controlled Components ⭐ (Recommended)

└── Uncontrolled Components
```

---

# Controlled Components (Most Important)

> [!TIP]
> A controlled component is an input whose value is controlled by React state.

```
Input

↓

onChange

↓

setState()

↓

React State

↓

value

↓

Input
```

---

# Example

```jsx
import { useState } from "react";

function LoginForm() {

    const [name, setName] = useState("");

    function handleChange(event) {

        setName(event.target.value);

    }

    return (

        <input

            value={name}

            onChange={handleChange}

        />

    );

}
```

---

# Working Flow

```
User types "A"

↓

onChange fires

↓

event.target.value

↓

setName("A")

↓

State Updated

↓

Component Re-render

↓

Input Shows "A"
```

---

# Why value + onChange?

Both are required.

```jsx
<input

value={name}

onChange={handleChange}

/>
```

Without

```jsx
onChange
```

Input becomes

```
Read Only
```

---

# Form Submission

```jsx
function Form(){

const [name,setName]=useState("");

function handleSubmit(event){

event.preventDefault();

console.log(name);

}

return(

<form onSubmit={handleSubmit}>

<input

value={name}

onChange={(e)=>setName(e.target.value)}

/>

<button>

Submit

</button>

</form>

);

}
```

---

# Why preventDefault()?

Normally

```
Submit

↓

Browser Refreshes Page
```

React

```
Submit

↓

preventDefault()

↓

Handle Data

↓

Call API

↓

Stay on Same Page
```

---

# Multiple Inputs

Instead of many states

```jsx
const [name,setName]=useState("");

const [email,setEmail]=useState("");

const [phone,setPhone]=useState("");
```

Use one object

```jsx
const [form,setForm]=useState({

name:"",

email:"",

phone:""

});
```

---

# Generic Input Handler

```jsx
function handleChange(e){

setForm({

...form,

[e.target.name]:e.target.value

});

}
```

Inputs

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

# How It Works

```
name="email"

↓

event.target.name

↓

"email"

↓

event.target.value

↓

abc@gmail.com

↓

State Updated
```

---

# Checkbox Example

```jsx
const [checked,setChecked]=useState(false);

<input

type="checkbox"

checked={checked}

onChange={(e)=>setChecked(e.target.checked)}

/>
```

---

# Radio Button Example

```jsx
const [gender,setGender]=useState("");

<input

type="radio"

value="Male"

checked={gender==="Male"}

onChange={(e)=>setGender(e.target.value)}

/>
```

---

# Select Example

```jsx
const [city,setCity]=useState("");

<select

value={city}

onChange={(e)=>setCity(e.target.value)}

>

<option>Pune</option>

<option>Mumbai</option>

</select>
```

---

# Textarea Example

```jsx
const [message,setMessage]=useState("");

<textarea

value={message}

onChange={(e)=>setMessage(e.target.value)}

/>
```

---

# File Upload

Files cannot be fully controlled.

Use

```jsx
useRef()
```

or

```jsx
event.target.files
```

Example

```jsx
<input

type="file"

onChange={(e)=>{

console.log(e.target.files[0]);

}}

/>
```

---

# Controlled vs Uncontrolled Components

## Controlled

```
React State

↓

Input
```

```jsx
<input

value={name}

onChange={handleChange}

/>
```

---

## Uncontrolled

```
DOM

↓

Input

↓

React Reads Value Later
```

```jsx
const inputRef=useRef();

<input ref={inputRef}/>
```

---

# Controlled vs Uncontrolled

| Controlled | Uncontrolled |
|------------|--------------|
| React stores value | DOM stores value |
| Uses useState | Uses useRef |
| Easy validation | Less control |
| Preferred | Used for simple forms or file inputs |

---

# Form Validation

Example

```jsx
if(name===""){

alert("Required");

}
```

Email validation

```jsx
if(!email.includes("@")){

alert("Invalid Email");

}
```

---

# Async Form Submission

```jsx
async function handleSubmit(e){

e.preventDefault();

await fetch("/users",{

method:"POST",

body:JSON.stringify(form)

});

}
```

---

# Loading State

```jsx
const [loading,setLoading]=useState(false);

async function submit(){

setLoading(true);

await saveUser();

setLoading(false);

}
```

---

# Error State

```jsx
const [error,setError]=useState("");

try{

await save();

}

catch{

setError("Something went wrong");

}
```

---

# Reset Form

```jsx
setForm({

name:"",

email:"",

phone:""

});
```

---

# Best Libraries

Most companies use

- React Hook Form ⭐⭐⭐⭐⭐
- Formik
- Yup
- Zod

---

# Why React Hook Form?

Advantages

- Less re-rendering
- Better performance
- Easy validation
- Small bundle
- Works well with TypeScript

---

# React Hook Form Example

```jsx
import { useForm } from "react-hook-form";

function Login(){

const {

register,

handleSubmit

}=useForm();

function onSubmit(data){

console.log(data);

}

return(

<form onSubmit={handleSubmit(onSubmit)}>

<input

{...register("username")}

/>

<button>

Login

</button>

</form>

);

}
```

---

# Form Validation with React Hook Form

```jsx
<input

{...register("email",{

required:true

})}

/>
```

---

# Common Interview Mistakes

## Wrong

```jsx
<input

value={name}

/>
```

No

```jsx
onChange
```

Input becomes read-only.

---

## Correct

```jsx
<input

value={name}

onChange={(e)=>setName(e.target.value)}

/>
```

---

# Best Practices

- Prefer controlled components for most forms.
- Keep form state together in a single object when handling multiple fields.
- Use a generic `handleChange` for scalability.
- Always call `event.preventDefault()` when handling form submission.
- Use `React Hook Form` for large or complex forms.
- Validate input on both the client and the server.
- Display loading and error states during asynchronous submissions.

---

# Interview Questions

## Beginner

### What is a controlled component?

An input whose value is controlled by React state.

---

### Why do we use onChange?

To update React state whenever the input value changes.

---

### Why do we use value?

To bind the input to React state.

---

### What is preventDefault()?

It prevents the browser's default form submission and page reload.

---

## Intermediate

### Controlled vs Uncontrolled Components?

| Controlled | Uncontrolled |
|------------|--------------|
| React manages state | DOM manages state |
| Uses useState | Uses useRef |
| Recommended | Limited use cases |

---

### Why is React Hook Form faster?

Because it minimizes unnecessary re-renders by relying on uncontrolled inputs internally while still providing a declarative API.

---

### How do you manage multiple inputs?

Store all fields in one state object and update them with a generic change handler using the input's `name` attribute.

---

## Senior-Level Questions

### How would you build a large enterprise form?

- React Hook Form
- Zod/Yup validation
- Reusable input components
- Centralized error handling
- Loading and submission states
- API abstraction
- Optimistic UI where appropriate

---

### How do you prevent unnecessary re-renders in forms?

- Use React Hook Form or uncontrolled inputs where suitable.
- Split large forms into smaller components.
- Memoize expensive child components.
- Avoid storing derived values in state.

---

### When would you use uncontrolled components?

- File inputs
- Simple forms
- Integrating third-party libraries
- Performance-sensitive scenarios with many fields

---

# Scenario-Based Questions

## Scenario 1

You have 20 input fields.

**Answer**

Use

- One form state object
- Generic `handleChange`
- Or React Hook Form

---

## Scenario 2

You need validation for email and password.

**Answer**

Use React Hook Form with Yup or Zod.

---

## Scenario 3

You are uploading files.

**Answer**

Use

```jsx
<input type="file" />
```

Access the file with `event.target.files` (or a ref). File inputs are not controlled like text inputs.

---

## Scenario 4

The interviewer asks:

**Why are controlled components recommended?**

**Answer**

Because React becomes the single source of truth, making validation, conditional rendering, debugging, and synchronization much easier.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Preferred Approach | Controlled Components |
| State Hook | `useState()` |
| Submit Handler | `onSubmit` |
| Prevent Reload | `event.preventDefault()` |
| Multiple Fields | Single state object + generic handler |
| Validation | Manual or React Hook Form |
| Large Forms | React Hook Form + Yup/Zod |
| File Upload | `event.target.files` or `useRef()` |

---

# Excalidraw Mind Map

```text
                    React Forms

                         │

         ┌───────────────┼────────────────┐

         │               │                │

 Controlled        Uncontrolled      React Hook Form

         │               │                │

      useState        useRef         register()

         │               │          handleSubmit()

         │               │          validation

         │               │

         ├───────────────┐

         │               │

   Input Events      Form Submit

         │               │

     onChange      preventDefault()

         │               │

     Update State     API Request

         │               │

     Re-render      Loading/Error

         │

  Validation

         │

 Generic Handler

         │

 Multiple Inputs

         │

 Single Source of Truth
```

---

# Related Notes

- [[useState]]
- [[useRef]]
```