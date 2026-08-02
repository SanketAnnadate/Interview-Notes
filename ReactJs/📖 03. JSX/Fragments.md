---
title: What are Fragments in React?
tags:
  - react
  - fragments
  - jsx
  - interview
  - react-basics
created: 2026-08-02
---

# What are Fragments in React?

> [!INFO]
> **Fragments** are a React feature that allows you to group multiple JSX elements **without adding an extra DOM element** like `<div>`.

> [!IMPORTANT]
> A Fragment exists only during rendering.
>
> It **does not create an additional HTML element** in the browser.

---

# Beginner Explanation

Imagine you have two books.

Normally you keep them inside a bag.

```
Bag

├── Book 1

└── Book 2
```

The bag is an extra container.

Sometimes you don't need the bag.

You just carry the books.

```
Book 1

Book 2
```

A **Fragment** is like carrying the books **without adding the bag**.

---

# Why Do We Need Fragments?

React components must return **only one parent element**.

This is invalid.

```jsx
function App() {

    return (

        <h1>Hello</h1>

        <p>Welcome</p>

    );

}
```

React throws an error because multiple root elements are returned.

---

## Solution 1 (Using div)

```jsx
function App() {

    return (

        <div>

            <h1>Hello</h1>

            <p>Welcome</p>

        </div>

    );

}
```

Generated HTML

```html
<div>

    <h1>Hello</h1>

    <p>Welcome</p>

</div>
```

An unnecessary `<div>` is added.

---

## Solution 2 (Using Fragment)

```jsx
function App() {

    return (

        <React.Fragment>

            <h1>Hello</h1>

            <p>Welcome</p>

        </React.Fragment>

    );

}
```

Generated HTML

```html
<h1>Hello</h1>

<p>Welcome</p>
```

No extra DOM node.

---

# Short Syntax

Instead of

```jsx
<React.Fragment>

    <h1>Hello</h1>

</React.Fragment>
```

Use

```jsx
<>

    <h1>Hello</h1>

</>
```

This is the preferred syntax.

---

# Complete Example

```jsx
function User() {

    return (

        <>

            <h2>Sanket</h2>

            <p>Java Full Stack Developer</p>

        </>

    );

}

export default User;
```

Browser HTML

```html
<h2>Sanket</h2>

<p>Java Full Stack Developer</p>
```

No wrapper `<div>`.

---

# Why Avoid Extra div?

Suppose

```jsx
<div>

    <div>

        <div>

            <div>

                Content

            </div>

        </div>

    </div>

</div>
```

This is called

```
Div Soup
```

Too many unnecessary `<div>` elements make HTML harder to read and maintain.

Fragments solve this problem.

---

# Internal Working

Without Fragment

```
JSX

↓

<div>

↓

Virtual DOM

↓

Real DOM

↓

Extra DOM Node
```

---

With Fragment

```
JSX

↓

Fragment

↓

Virtual DOM

↓

Fragment Removed

↓

Only Child Elements Rendered
```

---

# Fragment vs div

| Fragment | div |
|-----------|-----|
| Doesn't create DOM node | Creates DOM node |
| Better Performance | Extra DOM Element |
| Cleaner HTML | More Nested HTML |
| No CSS Styling | Can Apply CSS |
| No Layout Impact | Affects Layout |

---

# When Should You Use Fragment?

Use Fragment when

- Returning multiple JSX elements
- Avoiding unnecessary `<div>`
- Rendering table rows
- Rendering list items
- Creating reusable components
- Keeping HTML clean

---

# When Should You Use div?

Use `<div>` when

- CSS styling is required
- Flexbox/Grid container is needed
- Margin or padding is required
- Event handlers are needed
- Class names or IDs are required

---

# Fragment with Key

Short syntax

```jsx
<>

    ...

</>
```

cannot receive props.

Suppose you render a list.

Wrong

```jsx
items.map(item => (

    <>

        <h2>{item.name}</h2>

    </>

));
```

Correct

```jsx
items.map(item => (

    <React.Fragment

        key={item.id}

    >

        <h2>{item.name}</h2>

    </React.Fragment>

));
```

Only `React.Fragment` supports the `key` prop.

---

# Real World Example

## Table

Wrong

```jsx
<table>

    <div>

        <tr>

            <td>John</td>

        </tr>

    </div>

</table>
```

Invalid HTML.

Correct

```jsx
<table>

    <>

        <tr>

            <td>John</td>

        </tr>

    </>

</table>
```

---

## Layout Component

```jsx
function Layout() {

    return (

        <>

            <Navbar />

            <Dashboard />

            <Footer />

        </>

    );

}
```

No unnecessary wrapper element.

---

# Advantages of Fragments

✅ No unnecessary DOM elements

✅ Cleaner HTML

✅ Better readability

✅ Prevents "Div Soup"

✅ Slightly better performance

✅ Useful for tables and lists

✅ Keeps component hierarchy clean

---

# Disadvantages

- Cannot apply CSS classes.
- Cannot attach event handlers.
- Short syntax (`<>...</>`) cannot have a `key`.
- Cannot receive attributes except `key` on `React.Fragment`.

---

# Common Mistakes

## Adding div Everywhere

Wrong

```jsx
<div>

    <Header />

</div>

<div>

    <Footer />

</div>
```

Better

```jsx
<>

    <Header />

    <Footer />

</>
```

---

## Using Short Fragment with key

Wrong

```jsx
<>

    ...

</>
```

inside `map()`.

Correct

```jsx
<React.Fragment

    key={id}

>

    ...

</React.Fragment>
```

---

# Best Practices

- Prefer Fragment over unnecessary `<div>`.
- Use short syntax (`<>...</>`) when no `key` is needed.
- Use `React.Fragment` when rendering lists with `key`.
- Use `<div>` only when styling or layout is required.

---

# Interview Questions

## Beginner

### What is a Fragment?

A Fragment allows multiple JSX elements to be grouped without creating an extra DOM node.

---

### Why do we use Fragments?

To avoid unnecessary wrapper elements while returning multiple JSX elements.

---

### Does Fragment appear in HTML?

No.

Fragments are removed during rendering.

---

### What is the shorthand syntax?

```jsx
<>

</>
```

---

# Intermediate

### Difference between Fragment and div?

| Fragment | div |
|-----------|-----|
| No DOM node | DOM node created |
| No styling | Styling supported |
| Better HTML | Extra HTML |

---

### Can Fragment receive props?

No.

Only

```jsx
<React.Fragment

    key={...}

>
```

supports the `key` prop.

---

### Why are Fragments useful in tables?

HTML tables cannot contain arbitrary wrapper elements like `<div>` around `<tr>` elements. Fragments allow grouping without breaking valid HTML structure.

---

# Senior-Level Interview Questions

### Do Fragments improve performance?

Slightly.

They reduce unnecessary DOM nodes, leading to a cleaner DOM tree. However, the main benefit is improved HTML structure and maintainability rather than large performance gains.

---

### Why can't the shorthand Fragment syntax accept a key?

Because the shorthand syntax is only syntactic sugar. When a `key` is required (such as inside `map()`), React requires the explicit `React.Fragment` element.

---

### When should you prefer a div over a Fragment?

Use a `<div>` when you need:

- CSS classes
- IDs
- Styling
- Layout (Flexbox/Grid)
- Event handlers
- DOM references (`ref`)

---

# Scenario-Based Questions

## Scenario 1

Your HTML contains many nested `<div>` elements with no styling or behavior.

How would you improve it?

**Answer**

Replace unnecessary wrapper `<div>` elements with React Fragments.

---

## Scenario 2

You are rendering a list.

```jsx
items.map(item => (

    <>

        <Card />

    </>

));
```

React shows a key warning.

Why?

**Answer**

The shorthand Fragment syntax cannot receive a `key`.

Use

```jsx
<React.Fragment

    key={item.id}

>

    <Card />

</React.Fragment>
```

---

## Scenario 3

An interviewer asks:

**"Do Fragments improve React performance?"**

**Answer**

Fragments avoid unnecessary DOM nodes, resulting in a cleaner DOM structure. While this can provide minor performance benefits, their primary advantage is cleaner markup and better component composition.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Fragment | Groups JSX without extra DOM |
| DOM Node Created | ❌ No |
| Shorthand | `<>...</>` |
| Supports key | Only `React.Fragment` |
| Used For | Multiple JSX elements |
| Better Than div? | When no wrapper is needed |

---

# Excalidraw Diagram

```text
Without Fragment

Component

        │

        ▼

      <div>

      /   \

   <h1>  <p>

        │

        ▼

Extra DOM Node



With Fragment

Component

        │

        ▼

 Fragment

     /     \

  <h1>     <p>

        │

        ▼

No Extra DOM Node
```

---

# Related Notes

- [[📖 00. ReactJs]]