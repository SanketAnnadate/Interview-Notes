# How to Export and Import Components in React?

> [!INFO] Definition
> React applications are built using multiple components stored in different files. To use a component in another file, we **export** it from one file and **import** it into another.

---

# Why Do We Need Export and Import?

In a real-world React application, every component is usually placed in its own file.

Example Project Structure:

```text
src/
│
├── App.jsx
├── components/
│   ├── Navbar.jsx
│   ├── Footer.jsx
│   ├── Home.jsx
│   ├── Login.jsx
│   └── Dashboard.jsx
```

To use `Navbar` inside `App`, we must export it from `Navbar.jsx` and import it into `App.jsx`.

---

# Types of Exports

React (JavaScript ES6 Modules) supports two types of exports:

1. Default Export
2. Named Export

---

# 1. Default Export

A file can have **only one default export**.

### Home.jsx

```jsx
function Home() {
    return <h1>Home Component</h1>;
}

export default Home;
```

### App.jsx

```jsx
import Home from "./Home";

function App() {
    return (
        <>
            <Home />
        </>
    );
}

export default App;
```

---

## Characteristics of Default Export

- Only **one** default export per file.
- Import name can be anything.

Example:

```jsx
import HomePage from "./Home";
```

or

```jsx
import Dashboard from "./Home";
```

Although this works, use meaningful names to improve readability.

---

# 2. Named Export

Named exports allow **multiple exports** from the same file.

### Utils.js

```javascript
export const add = (a, b) => a + b;

export const subtract = (a, b) => a - b;

export const multiply = (a, b) => a * b;
```

### App.jsx

```javascript
import { add, subtract, multiply } from "./Utils";

console.log(add(10, 20));
```

---

## Characteristics of Named Export

- Multiple exports allowed.
- Import names **must match** the exported names.
- Curly braces `{}` are required.

---

# Exporting Multiple Components

```jsx
export function Header() {
    return <h1>Header</h1>;
}

export function Footer() {
    return <h1>Footer</h1>;
}
```

Import:

```jsx
import { Header, Footer } from "./Layout";
```

---

# Combining Default and Named Exports

```jsx
export const version = "1.0";

export function Header() {
    return <h1>Header</h1>;
}

export default function App() {
    return <h1>App</h1>;
}
```

Import:

```jsx
import App, { Header, version } from "./App";
```

---

# Renaming Named Imports

Use the `as` keyword.

```javascript
export const add = (a, b) => a + b;
```

Import:

```javascript
import { add as sum } from "./Utils";

console.log(sum(10, 20));
```

---

# Import Everything

```javascript
import * as Utils from "./Utils";

console.log(Utils.add(10, 20));
console.log(Utils.subtract(20, 10));
```

---

# Relative Paths

Current folder:

```jsx
import Home from "./Home";
```

Parent folder:

```jsx
import Navbar from "../Navbar";
```

Nested folder:

```jsx
import Login from "./pages/Login";
```

---

# Default Export vs Named Export

| Feature | Default Export | Named Export |
|---------|----------------|--------------|
| Number Allowed | One | Multiple |
| Curly Braces Required | ❌ No | ✅ Yes |
| Name Can Change | ✅ Yes | ❌ No (unless using `as`) |
| Syntax | `export default` | `export` |
| Best Use | Components | Utility functions, constants, helpers |

---

# Real-World Example

```text
Banking Application

App.jsx

│

├── Navbar.jsx

├── Dashboard.jsx

├── Transaction.jsx

├── AccountSummary.jsx

├── Loan.jsx

└── Footer.jsx
```

App.jsx

```jsx
import Navbar from "./components/Navbar";
import Dashboard from "./components/Dashboard";
import Footer from "./components/Footer";

function App() {
    return (
        <>
            <Navbar />
            <Dashboard />
            <Footer />
        </>
    );
}

export default App;
```

Each component is developed independently and then assembled in `App`.

---

# Common Mistakes

### ❌ Forgetting `default`

```jsx
function Home() {}

export Home;
```

✅ Correct

```jsx
export default Home;
```

---

### ❌ Missing Curly Braces for Named Export

```jsx
import add from "./Utils";
```

✅ Correct

```jsx
import { add } from "./Utils";
```

---

### ❌ Using Curly Braces with Default Export

```jsx
import { Home } from "./Home";
```

✅ Correct

```jsx
import Home from "./Home";
```

---

### ❌ Incorrect File Path

```jsx
import Home from "./Pages/Home";
```

Ensure the file path and filename are correct.

---

# Best Practices

- Use **default exports** for React components.
- Use **named exports** for utility functions, constants, and helper methods.
- Keep one component per file.
- Use meaningful file and component names.
- Avoid mixing too many exports in one file unless they are closely related.

---

# Interview Questions

## Basic

### What is the purpose of export and import in React?

They allow components, functions, or variables defined in one file to be used in another, promoting modular and reusable code.

---

### What is the difference between default and named export?

- **Default Export:** Only one per file, imported without curly braces.
- **Named Export:** Multiple per file, imported using curly braces.

---

### Can a file have multiple default exports?

No. A file can have only one default export.

---

### Why are curly braces used with named exports?

They tell JavaScript exactly which named members to import from the module.

---

# Senior-Level Interview Questions

### When would you choose a named export over a default export?

Use named exports when a module exposes multiple related utilities, helper functions, or constants. This improves discoverability and allows editors to provide better auto-completion.

---

### Why do React projects usually use default exports for components?

Each file typically represents a single component, making a default export simple and easy to import with a clean syntax.

---

### Does React itself provide export/import?

No. `export` and `import` are part of the **ES6 JavaScript Modules** specification. React uses this language feature to organize components.

---

# Scenario-Based Questions

## Scenario 1

A developer writes:

```jsx
export default Header;

export default Footer;
```

**What is the issue?**

**Answer:**

A module can have only one default export.

---

## Scenario 2

You see this code:

```jsx
import { Home } from "./Home";
```

But `Home.jsx` contains:

```jsx
export default Home;
```

**Why does the application fail?**

**Answer:**

Default exports must be imported **without** curly braces.

Correct:

```jsx
import Home from "./Home";
```

---

## Scenario 3

A utility file exports multiple helper functions.

**Which export type should you use?**

**Answer:**

Named exports, because they allow multiple exports from the same file.

---

# Quick Revision

| Topic | Description |
|--------|-------------|
| `export default` | Export one main value from a file |
| `export` | Export multiple named values |
| `import Component from "./File"` | Import a default export |
| `import { name } from "./File"` | Import a named export |
| `import * as Utils` | Import all named exports as an object |
| `as` | Rename a named import |

---
