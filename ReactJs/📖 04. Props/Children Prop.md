---
title: What is Children Prop in React?
tags:
  - react
  - props
  - children
  - component-composition
  - interview
created: 2026-08-02
---

# What is Children Prop in React?

> [!INFO]
> **children** is a special prop provided automatically by React. It represents everything placed **between the opening and closing tags** of a component.

```jsx
<Component>

    {/* Everything here */}

</Component>
```

Everything inside the component tags becomes **props.children**.

---

# Beginner Explanation

Imagine you buy a gift box.

The box itself is fixed.

But you can put anything inside it.

```
Gift Box

↓

Chocolate

↓

Flowers

↓

Money

↓

Letter
```

The **Gift Box** is the React component.

The contents inside are **children**.

---

# Why Do We Need Children Prop?

Suppose you create a Card component.

Without children

```jsx
function Card() {

    return (

        <div className="card">

            Fixed Content

        </div>

    );

}
```

Every card looks the same.

---

With children

```jsx
function Card({

    children

}) {

    return (

        <div className="card">

            {children}

        </div>

    );

}
```

Now every Card can display different content.

---

# Basic Example

## Parent Component

```jsx
function App() {

    return (

        <Card>

            <h2>Welcome</h2>

            <p>React is awesome.</p>

        </Card>

    );

}
```

---

## Child Component

```jsx
function Card({

    children

}) {

    return (

        <div className="card">

            {children}

        </div>

    );

}

export default Card;
```

---

## Output

```
+-----------------------+

Welcome

React is awesome.

+-----------------------+
```

---

# What Does React Actually Do?

When React sees

```jsx
<Card>

    <h1>Hello</h1>

    <p>World</p>

</Card>
```

It converts it into

```jsx
<Card

    children={

        <>

            <h1>Hello</h1>

            <p>World</p>

        </>

    }

/>
```

So

```jsx
props.children
```

contains

```jsx
<>

<h1>Hello</h1>

<p>World</p>

</>
```

---

# Accessing Children

Using props

```jsx
function Container(props) {

    return (

        <div>

            {props.children}

        </div>

    );

}
```

---

Using Destructuring

```jsx
function Container({

    children

}) {

    return (

        <div>

            {children}

        </div>

    );

}
```

Preferred approach.

---

# Multiple Children

```jsx
<Card>

    <h2>Title</h2>

    <p>Description</p>

    <button>Read More</button>

</Card>
```

React stores all three elements inside

```jsx
children
```

---

# Children Can Be Anything

## HTML

```jsx
<Card>

    <h1>Hello</h1>

</Card>
```

---

## React Component

```jsx
<Card>

    <User />

</Card>
```

---

## Text

```jsx
<Card>

    Welcome

</Card>
```

---

## Button

```jsx
<Card>

    <button>

        Submit

    </button>

</Card>
```

---

## Multiple Components

```jsx
<Card>

    <Navbar />

    <Profile />

    <Footer />

</Card>
```

---

# Real World Example

## Modal Component

```jsx
<Modal>

    <h2>Delete Account?</h2>

    <p>This action cannot be undone.</p>

    <button>

        Delete

    </button>

</Modal>
```

Modal.jsx

```jsx
function Modal({

    children

}) {

    return (

        <div className="modal">

            {children}

        </div>

    );

}
```

Now the Modal becomes reusable.

---

# Layout Component

```jsx
<Layout>

    <Dashboard />

</Layout>
```

Layout.jsx

```jsx
function Layout({

    children

}) {

    return (

        <>

            <Navbar />

            {children}

            <Footer />

        </>

    );

}
```

Output

```
Navbar

Dashboard

Footer
```

The Layout controls the page structure while rendering different content through `children`.

---

# Why is Children Important?

Without children,

every reusable component would need separate props for each piece of content.

With children,

components become flexible and reusable.

Examples

- Card
- Modal
- Dialog
- Layout
- Sidebar
- Tabs
- Accordion
- Container
- Form

---

# Internal Working

```
Parent Component

↓

JSX

↓

React.createElement()

↓

children Prop Created

↓

Child Component Receives children

↓

Render children
```

---

# Common Mistakes

## Forgetting to Render Children

Wrong

```jsx
function Card() {

    return (

        <div>

            Card

        </div>

    );

}
```

Nothing inside `<Card>` appears.

---

Correct

```jsx
function Card({

    children

}) {

    return (

        <div>

            {children}

        </div>

    );

}
```

---

## Passing Data Using Children Instead of Props

Wrong

```jsx
<User>

John

</User>
```

Better

```jsx
<User

    name="John"

/>
```

Use `children` only when the content is intended to be nested inside the component.

---

# Children vs Props

| Props | Children |
|--------|----------|
| Explicitly passed | Automatically passed |
| Used for configuration | Used for nested content |
| `<User name="John" />` | `<Card><h1>Hello</h1></Card>` |
| Accessed as `props.name` | Accessed as `props.children` |

---

# Best Practices

- Use `children` for wrapper components.
- Prefer destructuring (`{ children }`).
- Keep wrapper components generic.
- Use children for layouts, modals, cards, and containers.
- Use named props for configuration data.

---

# Interview Questions

## Beginner

### What is the children prop?

`children` is a special prop that contains everything placed between a component's opening and closing tags.

---

### Who provides the children prop?

React automatically provides it.

---

### Is children a normal prop?

Yes.

It is a normal prop with the special name `children`.

---

### Can children contain multiple elements?

Yes.

It can contain

- Text
- HTML
- Components
- Arrays
- Fragments

---

# Intermediate

### Why do we use children?

To create reusable wrapper components such as Cards, Modals, Layouts, and Dialogs.

---

### Can children be another React component?

Yes.

Example

```jsx
<Card>

    <User />

</Card>
```

---

### How do you access children?

```jsx
function Card({

    children

}) {

    return children;

}
```

---

# Senior-Level Interview Questions

### Why is the children prop important for Component Composition?

React promotes **composition over inheritance**.

The `children` prop allows developers to build highly reusable container components without knowing in advance what content they will display.

---

### When should you use children instead of a prop?

Use `children` when the parent should decide **what content goes inside** a reusable wrapper.

Use named props when passing configuration or data.

---

### Can children improve component reusability?

Yes.

Components like Layout, Card, Modal, Tabs, and Dialog become reusable because they don't depend on specific content.

---

# Scenario-Based Questions

## Scenario 1

You need a reusable Modal component that can display different content on different pages.

How would you implement it?

**Answer**

Use the `children` prop.

```jsx
<Modal>

    <LoginForm />

</Modal>

<Modal>

    <DeleteConfirmation />

</Modal>
```

---

## Scenario 2

An interviewer asks:

"Why does React recommend composition over inheritance?"

**Expected Answer**

Composition allows components to be combined using props and `children`, making them more flexible, reusable, and easier to maintain than inheritance-based designs.

---

## Scenario 3

A Layout component should always display a Navbar and Footer, but the page content changes.

How would you design it?

**Answer**

```jsx
function Layout({

    children

}) {

    return (

        <>

            <Navbar />

            {children}

            <Footer />

        </>

    );

}
```

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| children | Special React prop |
| Purpose | Pass nested content |
| Automatically Passed | ✅ Yes |
| Can Contain | Text, JSX, Components, Arrays |
| Used In | Card, Modal, Layout, Dialog, Tabs |
| Access | `props.children` or `{ children }` |

---

# Related Notes

- [[Props]]