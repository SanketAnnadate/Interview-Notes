# How to Use Nested Components in React?

> [!INFO] Definition
> **Nested Components** are components rendered inside other components. React allows components to be composed together to build complex user interfaces from smaller, reusable building blocks.

---

# What are Nested Components?

A React application is built like a tree.

Instead of creating one large component, we combine multiple smaller components.

Example:

```text
App
│
├── Header
│   ├── Logo
│   ├── Navigation
│   └── UserProfile
│
├── Main
│   ├── Sidebar
│   ├── Dashboard
│   └── Statistics
│
└── Footer
```

Each component can contain other components.

This is called **Component Composition** or **Nested Components**.

---

# Why Use Nested Components?

Without nested components:

```text
App.jsx (2000+ lines)

Navbar

Sidebar

Dashboard

Cards

Charts

Footer
```

Everything is inside one file.

Problems:

- Difficult to maintain
- Difficult to debug
- Code duplication
- Poor readability

---

With nested components:

```text
App
│
├── Header
├── Dashboard
│   ├── Card
│   ├── Chart
│   └── Statistics
├── Profile
└── Footer
```

Advantages:

- Reusable
- Maintainable
- Modular
- Easy to Test
- Easy to Scale

---

# Example 1: Simple Nested Component

## Header.jsx

```jsx
function Header() {
    return (
        <header>
            <h1>Welcome</h1>
        </header>
    );
}

export default Header;
```

---

## Footer.jsx

```jsx
function Footer() {
    return (
        <footer>
            <h3>Copyright 2026</h3>
        </footer>
    );
}

export default Footer;
```

---

## App.jsx

```jsx
import Header from "./Header";
import Footer from "./Footer";

function App() {

    return (
        <>
            <Header />

            <h2>Main Content</h2>

            <Footer />
        </>
    );
}

export default App;
```

Output:

```text
--------------------
Welcome

Main Content

Copyright 2026
--------------------
```

---

# Example 2: Nested Component Inside Another Component

## Menu.jsx

```jsx
function Menu() {
    return (
        <nav>
            Home | About | Contact
        </nav>
    );
}

export default Menu;
```

---

## Header.jsx

```jsx
import Menu from "./Menu";

function Header() {

    return (
        <header>

            <h1>My Website</h1>

            <Menu />

        </header>
    );
}

export default Header;
```

---

## App.jsx

```jsx
import Header from "./Header";

function App() {

    return (
        <>
            <Header />
        </>
    );
}

export default App;
```

Component Hierarchy:

```text
App

↓

Header

↓

Menu
```

---

# Multi-Level Nesting

```text
App

↓

Dashboard

↓

Statistics

↓

Chart

↓

BarChart
```

React supports unlimited nesting levels, but avoid making the hierarchy unnecessarily deep.

---

# Passing Props to Nested Components

Nested components communicate using **Props**.

## UserCard.jsx

```jsx
function UserCard({ name }) {

    return <h2>{name}</h2>;

}

export default UserCard;
```

---

## Dashboard.jsx

```jsx
import UserCard from "./UserCard";

function Dashboard() {

    return (
        <>
            <UserCard name="Sanket" />

            <UserCard name="Rahul" />
        </>
    );
}

export default Dashboard;
```

---

# Real-World Banking Example

```text
Banking App

│

├── Navbar

├── Dashboard

│      │

│      ├── AccountSummary

│      ├── TransactionHistory

│      ├── CreditCard

│      └── Investment

│

├── Profile

└── Footer
```

The `Dashboard` component contains multiple child components, each responsible for a specific feature.

---

# Benefits of Nested Components

## 1. Reusability

The same component can be used in multiple places.

```jsx
<Card />

<Card />

<Card />
```

---

## 2. Better Maintainability

Changing one component automatically updates every place where it is used.

---

## 3. Easier Testing

Each component can be tested independently.

Example:

- Navbar Test
- Dashboard Test
- Footer Test

---

## 4. Improved Team Collaboration

Different developers can work on different components simultaneously.

Example:

Developer A → Navbar

Developer B → Dashboard

Developer C → Footer

---

## 5. Better Readability

Small focused components are easier to understand than one massive file.

---

# Best Practices

✅ Keep components small.

✅ One responsibility per component.

✅ Reuse components whenever possible.

✅ Avoid deeply nested component trees.

✅ Keep business logic separate from presentation when appropriate.

✅ Use meaningful component names.

---

# Common Mistakes

## ❌ Very Large Components

```jsx
App.jsx

3000 lines
```

Instead:

Split into:

```text
Navbar

Dashboard

Profile

Footer
```

---

## ❌ Duplicate UI

Instead of:

```jsx
<h2>User</h2>

<h2>User</h2>

<h2>User</h2>
```

Create:

```jsx
<UserCard />
```

---

## ❌ Deep Prop Drilling

```text
App

↓

Dashboard

↓

Card

↓

Button

↓

Icon
```

Passing props through many levels can become difficult to maintain.

Use:

- Context API
- Redux
- Zustand

when appropriate.

---

# Interview Questions

## Basic

### What are nested components?

Nested components are React components rendered inside other components to build complex UIs through composition.

---

### Why do we use nested components?

- Reusability
- Better organization
- Easier maintenance
- Improved readability
- Easier testing

---

### Can a component contain another component?

Yes.

React encourages composing components together.

---

### Is there a limit to nesting?

No technical limit, but deeply nested component trees can make applications harder to maintain.

---

# Senior-Level Interview Questions

### What is Component Composition?

Component Composition is the practice of building larger components by combining smaller reusable components instead of relying on inheritance.

React strongly favors **composition over inheritance**.

---

### How do you avoid deeply nested components?

- Split responsibilities.
- Use custom Hooks.
- Lift state only when necessary.
- Use Context API or state management libraries instead of excessive prop drilling.

---

### What problems arise with deep nesting?

- Prop drilling
- Difficult debugging
- Harder maintenance
- Reduced readability

---

# Scenario-Based Questions

## Scenario 1

Your `App.jsx` file has grown to over 2,500 lines.

**What would you do?**

**Answer:**

Break the application into smaller reusable components such as `Navbar`, `Sidebar`, `Dashboard`, `Footer`, and feature-specific components.

---

## Scenario 2

The same "User Card" appears on five different pages.

**How would you implement it?**

**Answer:**

Create a reusable `UserCard` component and pass different data through props instead of duplicating the markup.

---

## Scenario 3

A prop is passed through six levels of nested components before reaching the component that needs it.

**What issue is this called?**

**Answer:**

**Prop Drilling.**

Possible solutions:

- Context API
- Redux Toolkit
- Zustand

---

## Scenario 4

A child component needs to notify its parent when a button is clicked.

**How can this be achieved?**

**Answer:**

Pass a callback function from the parent to the child using props. The child invokes the callback when the event occurs.

---

# Quick Revision

| Topic | Description |
|--------|-------------|
| Nested Component | A component rendered inside another component |
| Component Composition | Building complex UIs by combining smaller components |
| Benefits | Reusability, readability, maintainability, scalability |
| Communication | Props (Parent → Child) |
| Child to Parent | Callback functions passed as props |
| Common Issue | Prop Drilling |
| Solution | Context API, Redux, Zustand |
| React Principle | Composition over Inheritance |

---

# Related Notes

