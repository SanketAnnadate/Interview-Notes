---
title: forwardRef
tags:
  - react
  - hooks
  - forwardref
  - refs
  - interview
  - advanced
created: 2026-08-03
---

# What is forwardRef?

> [!INFO]
> **forwardRef** is a React API that allows a **parent component** to pass a **ref** through a custom component and access its underlying DOM element or child component instance.

> [!IMPORTANT]
> Normally, **refs cannot be passed to custom components** because `ref` is a special React property. `forwardRef` solves this limitation.

---

# Beginner Explanation

Suppose you have this structure.

```
Parent

↓

Custom Component

↓

Input Box
```

The parent wants to focus the input.

Without `forwardRef`, the parent **cannot directly access** the input.

With `forwardRef`, React forwards the parent's ref to the child.

```
Parent

↓

forwardRef

↓

Child Component

↓

Input Element
```

---

# Why is forwardRef Needed?

Normally

```jsx
function MyInput() {

    return <input />;

}
```

Parent

```jsx
<MyInput ref={inputRef}/>
```

❌ This does **NOT** work.

Why?

Because `ref` is **not passed as a normal prop**.

---

# Solution

Wrap the component using

```jsx
forwardRef()
```

---

# Syntax

```jsx
const Component = forwardRef(

(props, ref) => {

    return (

        <input ref={ref}/>

    );

}

);
```

---

# Basic Example

### Child Component

```jsx
import { forwardRef } from "react";

const MyInput = forwardRef((props, ref) => {

    return (

        <input

            ref={ref}

            {...props}

        />

    );

});

export default MyInput;
```

---

### Parent Component

```jsx
import { useRef } from "react";
import MyInput from "./MyInput";

function App() {

    const inputRef = useRef(null);

    function focusInput() {

        inputRef.current.focus();

    }

    return (

        <>

            <MyInput ref={inputRef} />

            <button onClick={focusInput}>

                Focus

            </button>

        </>

    );

}
```

---

# Visual Representation

```text
Parent Component

↓

inputRef

↓

<MyInput ref={inputRef}>

↓

forwardRef()

↓

<input ref={inputRef}/>

↓

DOM Input

↓

focus()
```

---

# Internal Working

Without forwardRef

```text
Parent

↓

<MyInput>

↓

Input

×

Ref Stops Here
```

---

With forwardRef

```text
Parent

↓

<MyInput>

↓

forwardRef

↓

Input

↓

Ref Reaches DOM
```

---

# How forwardRef Works

React calls

```jsx
forwardRef(

(props, ref) => {

}
)
```

Notice

Instead of only

```jsx
(props)
```

React also passes

```jsx
ref
```

This ref is attached to the DOM element.

---

# Passing Props and Ref Together

```jsx
const Button = forwardRef(

(props, ref) => {

    return (

        <button

            ref={ref}

            {...props}

        />

    );

});
```

Both work

```jsx
<Button

ref={buttonRef}

disabled

className="primary"

/>
```

---

# Real World Use Cases

## Focus Input

```
Login Page

↓

Username Field

↓

Auto Focus
```

---

## Scroll to Element

```jsx
sectionRef.current.scrollIntoView();
```

---

## Text Editor

Focus editor from toolbar.

---

## Date Picker

Parent opens calendar.

---

## Modal

Parent opens or closes modal.

---

## Third-party Libraries

Libraries like

- Material UI
- Ant Design
- Chakra UI

internally use `forwardRef`.

---

# forwardRef + useRef

Most common interview question.

```jsx
const inputRef = useRef(null);

<MyInput ref={inputRef}/>
```

Inside child

```jsx
const MyInput = forwardRef(

(props, ref) => (

<input ref={ref}/>

));
```

---

# forwardRef + useImperativeHandle

Instead of exposing the entire DOM,

we expose only selected methods.

Example

```jsx
const MyInput = forwardRef(

(props, ref)=>{

const inputRef=useRef();

useImperativeHandle(ref,()=>({

focus(){

inputRef.current.focus();

}

}));

return(

<input ref={inputRef}/>

);

});
```

Parent

```jsx
inputRef.current.focus();
```

Cannot access the whole DOM.

Only

```jsx
focus()
```

is exposed.

---

# forwardRef vs Normal Props

| Props | Ref |
|--------|-----|
| Passed normally | Special React property |
| Read-only | Mutable reference |
| Used for data | Used for DOM access |

---

# forwardRef vs useRef

| useRef | forwardRef |
|----------|------------|
| Creates a ref | Passes a ref to child |
| Stores mutable values | Forwards ref through components |
| Used in parent | Used in child |

---

# Common Mistakes

## Wrong

```jsx
function MyInput(){

return <input/>;

}

<MyInput ref={ref}/>
```

Ref doesn't work.

---

## Correct

```jsx
const MyInput = forwardRef(

(props,ref)=>(

<input ref={ref}/>

));
```

---

# Best Practices

- Use `forwardRef` only when a parent truly needs access to a child DOM node.
- Keep component APIs declarative; avoid exposing DOM unless necessary.
- Prefer `useImperativeHandle` to expose a limited API instead of the entire DOM element.
- Don't use `forwardRef` for passing normal data—use props.

---

# Interview Questions

## Beginner

### What is forwardRef?

A React API that allows a parent component to pass a ref through a custom component to one of its child DOM elements.

---

### Why can't we attach a ref to a normal function component?

Because `ref` is a special React property and is not passed as a regular prop.

---

### Which Hook is usually used with forwardRef?

```jsx
useRef()
```

---

## Intermediate

### Difference between useRef and forwardRef?

| useRef | forwardRef |
|---------|------------|
| Creates a ref | Forwards a ref |
| Parent uses it | Child receives it |

---

### Can forwardRef pass refs through multiple components?

Yes.

Each intermediate component must also use `forwardRef`.

---

## Senior-Level Questions

### When should you use forwardRef?

When reusable components (inputs, buttons, modals, editors) need to expose their DOM node or imperative methods to a parent.

---

### Why is useImperativeHandle commonly paired with forwardRef?

To expose only specific methods (such as `focus()` or `open()`) instead of giving the parent unrestricted access to the DOM element.

---

### Is forwardRef still relevant in modern React?

Yes. Even with newer React patterns, it remains useful for reusable UI libraries and DOM access. (Some evolving React APIs may reduce its use in certain scenarios, but it's still an important interview topic.)

---

# Scenario-Based Questions

## Scenario 1

You built a reusable Input component.

The parent wants to focus it.

**Answer**

Use

```jsx
forwardRef
```

---

## Scenario 2

You created a reusable Modal.

The parent wants to open it.

**Answer**

Use

```jsx
forwardRef

+

useImperativeHandle
```

---

## Scenario 3

Material UI's TextField accepts a ref.

How?

**Answer**

Internally,

Material UI uses

```jsx
forwardRef()
```

---

## Scenario 4

The interviewer asks:

**Why not simply pass the ref as a prop?**

**Answer**

Because `ref` is a reserved React attribute. Function components do not receive it through `props`. `forwardRef` tells React how to pass it to the child.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| API | `forwardRef()` |
| Purpose | Forward a ref to a child |
| Used With | `useRef`, `useImperativeHandle` |
| Works On | Custom Components |
| Main Use | DOM Access |
| Common Examples | Inputs, Buttons, Modals, Editors |

---

# Excalidraw Mind Map

```text
                    forwardRef()

                         │

          Parent creates useRef()

                         │

                 <Child ref={ref}>

                         │

                  forwardRef()

                         │

          ┌──────────────┴──────────────┐

          │                             │

     DOM Element                 Child Component

          │                             │

      focus()                    scrollIntoView()

      click()                    play()

      select()                   measure()

          │

     useImperativeHandle

          │

  Expose Limited Methods

          │

     Better Encapsulation
```

---

# Related Notes

- [[Hooks]]
- [[useRef]]