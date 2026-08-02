# What is JSX?

> [!INFO] Definition
> **JSX (JavaScript XML)** is a syntax extension for JavaScript that allows developers to write **HTML-like code inside JavaScript**. JSX makes React components easier to write, read, and maintain.

> [!IMPORTANT]
> JSX is **not HTML**.
> It is **syntactic sugar** that is converted into JavaScript function calls by **Babel** during compilation.

---

# What does JSX stand for?

**JSX = JavaScript XML**

Although it looks like HTML, JSX is actually JavaScript.

Example:

```jsx
const element = <h1>Hello React</h1>;
```

The above code is **not understood by browsers directly**.

Babel converts it into:

```javascript
const element = React.createElement(
    "h1",
    null,
    "Hello React"
);
```

---

# Why was JSX introduced?

Before JSX, developers had to create UI using JavaScript functions.

Without JSX:

```javascript
const element = React.createElement(
    "h1",
    null,
    "Hello React"
);
```

With JSX:

```jsx
const element = <h1>Hello React</h1>;
```

The JSX version is:

- Easier to read
- Easier to write
- Less error-prone
- More maintainable

---

# How JSX Works Internally

```text
JSX Code

      │

      ▼

Babel Compiler

      │

      ▼

React.createElement()

      │

      ▼

Virtual DOM Object

      │

      ▼

React Fiber

      │

      ▼

Real DOM
```

---

# JSX Example

```jsx
function HelloWorld() {
    return (
        <h1>Hello, World!</h1>
    );
}

export default HelloWorld;
```

Babel converts it into:

```javascript
function HelloWorld() {
    return React.createElement(
        "h1",
        null,
        "Hello, World!"
    );
}
```

---

# JSX is an Expression

Since JSX becomes JavaScript, we can embed JavaScript expressions using `{}`.

Example:

```jsx
const name = "Sanket";

function App() {
    return <h1>Hello {name}</h1>;
}
```

Output

```text
Hello Sanket
```

---

# JavaScript inside JSX

```jsx
const age = 30;

function App() {

    return (
        <>
            <h1>Age : {age}</h1>

            <h2>{10 + 20}</h2>

            <h3>{age >= 18 ? "Adult" : "Minor"}</h3>
        </>
    );
}
```

---

# JSX Supports Expressions Only

Allowed:

```jsx
<h1>{10 + 20}</h1>

<h1>{name}</h1>

<h1>{isLogin ? "Yes" : "No"}</h1>
```

Not Allowed:

```jsx
<h1>
{
    if(age > 18){
        return "Adult";
    }
}
</h1>
```

Instead:

```jsx
const text = age > 18 ? "Adult" : "Minor";

return <h1>{text}</h1>;
```

---

# Rules of JSX

## 1. Only One Parent Element

❌ Wrong

```jsx
return (
    <h1>Hello</h1>

    <p>React</p>
);
```

✅ Correct

```jsx
return (
    <div>
        <h1>Hello</h1>
        <p>React</p>
    </div>
);
```

Or

```jsx
return (
    <>
        <h1>Hello</h1>
        <p>React</p>
    </>
);
```

---

## 2. Close Every Tag

✅

```jsx
<img src="logo.png" />

<input />

<br />
```

❌

```jsx
<img>

<input>
```

---

## 3. Use camelCase

HTML

```html
onclick
tabindex
maxlength
```

React

```jsx
onClick

tabIndex

maxLength
```

---

## 4. class becomes className

HTML

```html
<div class="container"></div>
```

React

```jsx
<div className="container"></div>
```

---

## 5. for becomes htmlFor

HTML

```html
<label for="name">
```

React

```jsx
<label htmlFor="name">
```

---

# JSX vs HTML

| HTML | JSX |
|-------|-----|
| class | className |
| for | htmlFor |
| onclick | onClick |
| tabindex | tabIndex |
| style="color:red" | style={{ color: "red" }} |
| HTML | JavaScript Expression |

---

# Advantages of JSX

- Easier to read
- Easier to write
- Better developer experience
- Supports JavaScript expressions
- Compile-time error detection
- Less boilerplate code
- Better IDE support
- Improved maintainability

---

# Disadvantages

- Requires Babel
- Beginners may confuse it with HTML
- Initial learning curve

---

# JSX vs React.createElement()

### JSX

```jsx
<h1>Hello React</h1>
```

### JavaScript

```javascript
React.createElement(
    "h1",
    null,
    "Hello React"
);
```

---

# JSX and Components

JSX can render components.

```jsx
function Header() {
    return <h1>Header</h1>;
}

function App() {
    return (
        <>
            <Header />
        </>
    );
}
```

---

# Fragment

Instead of unnecessary `<div>` elements:

```jsx
<>
    <Navbar />
    <Dashboard />
    <Footer />
</>
```

React Fragment avoids creating extra DOM nodes.

---

# Common Mistakes

❌ Using `class`

```jsx
<div class="box">
```

✅

```jsx
<div className="box">
```

---

❌ Returning multiple root elements

```jsx
return (
<h1>Hello</h1>
<p>World</p>
)
```

✅

```jsx
return (
<>
<h1>Hello</h1>
<p>World</p>
</>
)
```

---

❌ Using JavaScript statements inside JSX

```jsx
{
if(true){}
}
```

Use expressions instead.

---

# Real-World Example

A banking dashboard displays account details.

```jsx
function AccountCard({ user }) {
    return (
        <div className="card">
            <h2>{user.name}</h2>

            <p>Balance : ₹{user.balance}</p>

            <p>Status : {user.status}</p>
        </div>
    );
}
```

JSX keeps the UI readable while allowing JavaScript expressions inside the markup.

---

# Interview Questions

## Basic

### What is JSX?

JSX (JavaScript XML) is a syntax extension that allows developers to write HTML-like code inside JavaScript. It is compiled by Babel into `React.createElement()` calls.

---

### Is JSX mandatory in React?

No.

React can be written using `React.createElement()`, but JSX makes the code much easier to read and maintain.

---

### Is JSX HTML?

No.

JSX resembles HTML but is actually JavaScript syntax that gets compiled into JavaScript.

---

### Who converts JSX into JavaScript?

**Babel**.

---

### What does Babel do?

Babel transpiles modern JavaScript and JSX into browser-compatible JavaScript.

---

# Senior-Level Interview Questions

### Why is JSX preferred over `React.createElement()`?

- Better readability
- Better maintainability
- Less boilerplate
- Improved developer productivity
- Easier debugging

---

### Does JSX improve application performance?

No.

JSX is only syntax. Performance improvements come from React's Virtual DOM, Fiber architecture, and reconciliation process—not from JSX itself.

---

### What happens when Babel encounters JSX?

1. Parses JSX.
2. Converts it into `React.createElement()` calls.
3. Creates Virtual DOM objects.
4. React compares them during reconciliation.
5. Updates the Real DOM efficiently.

---

# Scenario-Based Questions

## Scenario 1

A developer writes:

```jsx
<div class="container">
```

Why is this incorrect?

**Answer:**

`class` is a reserved JavaScript keyword. React uses `className` instead.

---

## Scenario 2

A component returns:

```jsx
return (
<h1>Hello</h1>
<p>World</p>
);
```

What error will occur?

**Answer:**

JSX requires a single parent element. Wrap both elements inside a `<div>` or a React Fragment (`<>...</>`).

---

## Scenario 3

A developer writes:

```jsx
{
if(isLogin){
return "Welcome";
}
}
```

Why does this fail?

**Answer:**

JSX accepts only expressions, not statements like `if`. Use a ternary operator or compute the value before returning JSX.

---

# Best Practices

- Prefer JSX over `React.createElement()`.
- Keep JSX clean and readable.
- Move complex logic outside JSX.
- Use fragments instead of unnecessary `<div>` wrappers.
- Use descriptive component names.
- Avoid deeply nested JSX structures.
- Keep components focused on a single responsibility.

---

# Quick Revision

| Topic | Description |
|--------|-------------|
| JSX | JavaScript XML |
| Converted By | Babel |
| Output | `React.createElement()` |
| Looks Like | HTML |
| Actually Is | JavaScript |
| Supports | JavaScript Expressions |
| Parent Element | Required |
| `class` | `className` |
| `for` | `htmlFor` |
| Purpose | Easier UI development |

