# What are Components in React?

> [!INFO] Definition
> **Components** are the fundamental building blocks of a React application. They are reusable, independent pieces of UI that encapsulate their own structure, logic, and behavior. Components can be combined (nested) to build complex and scalable user interfaces.

---

# Why Do We Need Components?

Before React, developers often wrote an entire webpage in a single HTML file, making applications difficult to maintain.

React solves this problem by breaking the UI into small, reusable components.

### Without Components

```text
Entire Web Page
│
├── Navbar
├── Sidebar
├── Main Content
├── User Profile
├── Notifications
├── Footer
```

Everything is managed in one large file, making it difficult to update and debug.

---

### With Components

```text
App
│
├── Navbar
├── Sidebar
├── Dashboard
│   ├── Card
│   ├── Chart
│   └── Statistics
├── Profile
├── Notification
└── Footer
```

Each component has its own responsibility, making the application modular and maintainable.

---

# Characteristics of Components

- Reusable
- Independent
- Composable (can be nested)
- Maintainable
- Testable
- Encapsulated (contains its own logic and UI)

---

# Types of Components

React has two types of components:

## 1. Functional Components (Recommended)

Functional components are JavaScript functions that return JSX.

### Example

```jsx
function Welcome() {
    return <h1>Welcome to React</h1>;
}

export default Welcome;
```

### Using Arrow Function

```jsx
const Welcome = () => {
    return <h1>Welcome to React</h1>;
};

export default Welcome;
```

### Advantages

- Simple and easy to read
- Better performance
- Supports Hooks
- Preferred in modern React
- Less boilerplate code

---

## 2. Class Components (Legacy)

Before React Hooks, class components were used to manage state and lifecycle methods.

### Example

```jsx
import React, { Component } from "react";

class Welcome extends Component {

    render() {
        return (
            <h1>Welcome to React</h1>
        );
    }
}

export default Welcome;
```

### Features

- Can use state
- Lifecycle methods
- More boilerplate code
- Rarely used in new projects

---

# Functional Components vs Class Components

| Feature | Functional Component | Class Component |
|----------|----------------------|-----------------|
| Syntax | Function | ES6 Class |
| State | Hooks (`useState`) | `this.state` |
| Lifecycle | Hooks (`useEffect`) | Lifecycle methods |
| Performance | Better | Slightly heavier |
| Readability | High | Moderate |
| Recommended | ✅ Yes | ❌ Legacy |

---

# Real-World Example

Imagine building an online banking application.

```text
Banking App
│
├── Login
├── Navbar
├── Account Summary
├── Transaction List
├── Transfer Money
├── Loan Section
├── Profile
└── Footer
```

Each section is implemented as a separate React component.

For example:

```jsx
function AccountSummary() {
    return (
        <div>
            <h2>Available Balance</h2>
            <p>₹50,000</p>
        </div>
    );
}
```

This component can be reused wherever the account summary needs to be displayed.

---

# Benefits of Components

## 1. Reusability

Write once and use multiple times.

```jsx
<Button text="Save" />

<Button text="Delete" />

<Button text="Update" />
```

---

## 2. Easy Maintenance

Updating a component updates every place where it is used.

---

## 3. Better Code Organization

Instead of a 5,000-line file:

```text
App
├── Navbar
├── Dashboard
├── Footer
```

Each component resides in its own file.

---

## 4. Easier Testing

Components can be tested individually using:

- Jest
- React Testing Library
- Cypress

---

## 5. Improved Team Collaboration

Different developers can work on different components simultaneously without conflicts.

---

# Component Lifecycle

### Functional Components

Managed using Hooks.

```text
Render

↓

useEffect()

↓

Cleanup
```

---

### Class Components

```text
constructor()

↓

render()

↓

componentDidMount()

↓

componentDidUpdate()

↓

componentWillUnmount()
```

---

# Best Practices

- Keep components small and focused.
- Each component should have a single responsibility.
- Avoid deeply nested components.
- Reuse components whenever possible.
- Use functional components for new development.
- Keep business logic separate from UI when appropriate.
- Use meaningful component names.

---

# Common Mistakes

❌ Creating one huge component.

❌ Duplicating UI instead of reusing components.

❌ Keeping too much state in one component.

❌ Passing props through many levels (prop drilling) without considering Context or state management.

❌ Using class components in new projects without a specific need.

---

# Interview Questions

## Basic

### What are components in React?

**Answer:**

Components are reusable, independent building blocks of a React application that encapsulate UI and logic. They allow developers to build complex user interfaces by composing smaller pieces.

---

### Why are components important?

- Reusability
- Maintainability
- Scalability
- Easier Testing
- Better Code Organization

---

### What are the types of components?

- Functional Components
- Class Components

---

### Which component type is recommended today?

Functional Components, because they support Hooks and have simpler syntax.

---

### Can one component use another component?

Yes. Components can be nested.

Example:

```jsx
function App() {
    return (
        <>
            <Navbar />
            <Dashboard />
            <Footer />
        </>
    );
}
```

---

# Scenario-Based Questions

## Scenario 1

A React component is over 1,500 lines long and contains multiple responsibilities.

**What would you do?**

### Expected Answer

- Split it into smaller reusable components.
- Follow the Single Responsibility Principle (SRP).
- Extract reusable UI into shared components.
- Move business logic into custom Hooks if appropriate.

---

## Scenario 2

You need the same "User Card" in five different pages.

**How would you implement it?**

### Expected Answer

Create one reusable `UserCard` component and pass different data using props.

---

## Scenario 3

Two developers are modifying the same large component, causing merge conflicts.

**How can React help?**

### Expected Answer

Break the large component into smaller independent components so each developer can work on separate files.

---

## Scenario 4

Your application contains hundreds of components.

How would you organize them?

### Expected Answer

Use a feature-based folder structure.

```text
src/
│
├── components/
│   ├── Button/
│   ├── Modal/
│   └── Table/
│
├── features/
│   ├── Dashboard/
│   ├── Profile/
│   └── Authentication/
│
├── hooks/
├── services/
├── pages/
└── utils/
```

---

# Senior-Level Interview Questions

### Why did React move from Class Components to Functional Components?

Functional components are simpler, easier to understand, support Hooks, improve code reuse through custom Hooks, and reduce boilerplate.

---

### Should every piece of UI be a separate component?

No. Components should represent logical, reusable units. Creating too many tiny components can make the codebase harder to navigate.

---

### How do you decide when to split a component?

Consider splitting when:
- The component has multiple responsibilities.
- Parts of the UI can be reused.
- The file becomes difficult to read or maintain.
- Testing individual functionality becomes challenging.

---

# Quick Revision

| Feature | Description |
|---------|-------------|
| Component | Reusable building block of UI |
| Functional Component | JavaScript function returning JSX |
| Class Component | ES6 class (legacy approach) |
| Reusability | Write once, use many times |
| Composition | Combine components to build complex UIs |
| Encapsulation | UI and logic are grouped together |
| Recommended | Functional Components with Hooks |

---

# Related Notes

- [[Nested Component]]