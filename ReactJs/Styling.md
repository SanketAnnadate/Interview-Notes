---
title: Styling in React
tags:
  - react
  - css
  - styling
  - interview
  - frontend
created: 2026-08-02
---

# How to Style Components in React?

> [!INFO]
> Styling in React refers to applying CSS rules to React components to control their appearance, layout, responsiveness, and animations.

React supports multiple ways of styling components. The best approach depends on the project's size, scalability requirements, and team preferences.

---

# Why Do We Need Styling?

Without CSS

```
Button

Text

Input
```

Everything looks plain.

With CSS

```
Rounded Buttons

Responsive Layout

Animations

Themes

Professional UI
```

---

# Different Ways to Style React Components

```
React Component

        │

        ▼

+---------------------------+
|   Styling Techniques      |
+---------------------------+
        │
        ├── Inline CSS
        ├── CSS Stylesheet
        ├── CSS Modules
        ├── Styled Components
        ├── Emotion
        ├── Tailwind CSS
        ├── Sass / SCSS
        └── Material UI
```

---

# 1. Inline Styling

CSS is written directly inside JSX.

Example

```jsx
function Button() {

    return (

        <button

            style={{

                backgroundColor: "blue",

                color: "white",

                padding: "10px",

                borderRadius: "8px"

            }}

        >

            Save

        </button>

    );

}
```

### Advantages

- Simple
- Dynamic styling
- JavaScript expressions supported

### Disadvantages

- No pseudo classes (`:hover`)
- No media queries
- Difficult to maintain

---

# 2. External CSS Stylesheet

styles.css

```css
.button {

    background: blue;

    color: white;

    padding: 10px;

    border-radius: 8px;

}
```

Button.jsx

```jsx
import "./styles.css";

function Button() {

    return (

        <button className="button">

            Save

        </button>

    );

}
```

### Advantages

- Easy to maintain
- Reusable
- Standard CSS

### Disadvantages

- Global namespace
- Class name conflicts

---

# 3. CSS Modules

Button.module.css

```css
.button {

    background: blue;

    color: white;

}
```

Button.jsx

```jsx
import styles from "./Button.module.css";

function Button() {

    return (

        <button

            className={styles.button}

        >

            Save

        </button>

    );

}
```

Generated HTML

```html
<button class="button_xY34K">
```

### Advantages

- No naming conflicts
- Scoped CSS
- Preferred for medium/large applications

---

# 4. Styled Components (CSS-in-JS)

Install

```bash
npm install styled-components
```

Example

```jsx
import styled from "styled-components";

const Button = styled.button`

    background: blue;

    color: white;

    padding: 10px;

`;

function App() {

    return <Button>Save</Button>;

}
```

### Advantages

- Component scoped
- Dynamic styling
- No CSS files

### Disadvantages

- Additional dependency
- Slight runtime overhead

---

# 5. Tailwind CSS (Most Popular)

```jsx
function Button() {

    return (

        <button

            className="bg-blue-600 text-white px-4 py-2 rounded"

        >

            Save

        </button>

    );

}
```

### Advantages

- Very fast development
- Utility-first
- Excellent performance
- Responsive classes
- Industry standard

### Companies

- Google
- Netflix
- Vercel
- GitHub

---

# 6. Sass / SCSS

```scss
$primary: blue;

.button {

    background: $primary;

    color: white;

}
```

Supports

- Variables
- Mixins
- Nesting
- Functions

---

# 7. Emotion

Very similar to Styled Components.

```jsx
/** @jsxImportSource @emotion/react */

import { css } from "@emotion/react";

<button

    css={css`

        color: white;

        background: blue;

    `}

>

    Save

</button>
```

---

# 8. Material UI Styling

```jsx
<Button

    variant="contained"

>

    Save

</Button>
```

Provides

- Themes
- Components
- Responsive UI

---

# Dynamic Styling

```jsx
function Button({

    active

}) {

    return (

        <button

            style={{

                backgroundColor:

                    active

                        ? "green"

                        : "gray"

            }}

        >

            Submit

        </button>

    );

}
```

---

# Conditional Styling

```jsx
<div

    className={

        isDark

            ? "dark"

            : "light"

    }

>

</div>
```

---

# Using clsx

```jsx
import clsx from "clsx";

<div

    className={clsx(

        "card",

        active && "active"

    )}

>
```

Very common in enterprise projects.

---

# Responsive Styling

Tailwind

```jsx
<div

    className="

        text-sm

        md:text-lg

        lg:text-xl

    "

>

Hello

</div>
```

---

# CSS Variables

```css
:root {

    --primary: blue;

}

.button {

    background:

        var(--primary);

}
```

Useful for themes.

---

# Dark Theme

```css
body.dark {

    --background: black;

    --text: white;

}
```

---

# Theme Switching

```jsx
const [theme, setTheme] =

    useState("light");
```

Switch

```
Light

↓

Dark

↓

Light
```

---

# Styling Comparison

| Method | Scope | Performance | Recommended |
|---------|-------|-------------|-------------|
| Inline | Component | Good | Small UI |
| CSS File | Global | Excellent | Small Apps |
| CSS Modules | Component | Excellent | Large Apps |
| Styled Components | Component | Good | Medium Apps |
| Emotion | Component | Good | Medium Apps |
| Tailwind CSS | Utility | Excellent | ⭐ Most Recommended |
| SCSS | Global | Excellent | Enterprise |
| Material UI | Component | Excellent | Dashboard Apps |

---

# Internal Working

```
React Component

↓

JSX

↓

className

↓

CSS

↓

Browser Style Engine

↓

Layout

↓

Paint

↓

Display
```

---

# Real World Project Structure

```
src

├── components

│      ├── Button

│      │      ├── Button.jsx

│      │      └── Button.module.css

│

├── pages

├── layouts

├── assets

├── styles

│      ├── global.css

│      ├── variables.css

│      └── theme.css

└── App.jsx
```

---

# Best Practices

✅ Prefer CSS Modules or Tailwind for large projects.

✅ Keep styles close to components.

✅ Avoid inline styles except for dynamic values.

✅ Use CSS variables for themes.

✅ Follow consistent naming conventions.

✅ Avoid global CSS where possible.

---

# Interview Questions

## Beginner

### How can we style React components?

- Inline CSS
- External CSS
- CSS Modules
- Styled Components
- Tailwind CSS
- Sass
- Emotion

---

### Why do we use `className` instead of `class`?

`class` is a reserved keyword in JavaScript.

React uses `className`.

---

### Why is inline styling written as an object?

Because JSX is JavaScript.

```jsx
style={{

    color: "red"

}}
```

---

# Intermediate

### What are CSS Modules?

CSS Modules scope CSS locally, preventing class name collisions by generating unique class names.

---

### CSS Modules vs Styled Components?

| CSS Modules | Styled Components |
|-------------|-------------------|
| CSS file | CSS-in-JS |
| Build-time | Runtime |
| Faster | More dynamic |
| No extra dependency | Extra dependency |

---

### When should you use inline styles?

For dynamic values such as colors, widths, or positioning calculated in JavaScript—not for entire component styling.

---

# Senior-Level Interview Questions

### Which styling solution would you choose for a large enterprise application?

A common combination is:

- **Tailwind CSS** for utility classes and rapid development.
- **CSS Modules** for component-specific styles where utility classes become complex.
- **CSS Variables** for theming (light/dark mode).
- **Design system** (e.g., Material UI or custom components) for consistency.

The choice depends on the team's standards and the project's needs.

---

### Why is Tailwind popular?

- No naming conflicts
- Utility-first approach
- Excellent tree-shaking
- Easy responsive design
- Consistent spacing and colors
- Highly productive for large teams

---

# Scenario-Based Questions

## Scenario 1

Your application has hundreds of components, and global CSS class names are conflicting.

**Solution**

Use CSS Modules or a CSS-in-JS solution like Styled Components.

---

## Scenario 2

Your designer wants a Dark Mode toggle.

How would you implement it?

**Answer**

Use CSS variables (custom properties) or a theming solution (Tailwind dark mode, Material UI theme, etc.) and toggle a theme class or data attribute.

---

## Scenario 3

The background color of a button depends on an API response.

Which styling method is best?

**Answer**

Use conditional styling or inline styles for the dynamic property while keeping the rest of the styling in CSS Modules or Tailwind.

```jsx
<button

    style={{

        backgroundColor: statusColor

    }}

>

Save

</button>
```

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Inline CSS | Dynamic styling inside JSX |
| CSS File | Global stylesheet |
| CSS Modules | Scoped component CSS |
| Styled Components | CSS-in-JS |
| Tailwind CSS | Utility-first styling |
| SCSS | CSS with variables and nesting |
| Emotion | CSS-in-JS library |
| Material UI | Prebuilt component styling |

---

# Excalidraw Diagram

```text
                     Styling in React

                            │

      ┌─────────────────────┼─────────────────────┐

      │                     │                     │

 Inline CSS           CSS Files          CSS Modules

      │                     │                     │

 Dynamic              Global CSS        Scoped CSS

      │                     │                     │

      ├──────────────┬──────┴──────────────┐

      │              │                     │

 Styled Components  Tailwind CSS        Material UI

      │              │                     │

 CSS-in-JS      Utility Classes     Component Library

      │              │                     │

      └──────────────┴─────────────────────┘

                     Production UI
```

---

# Related Notes

- [[JSX]]
- [[Components]]
- [[Props]]
- [[Conditional Rendering]]
- [[Tailwind CSS]]
- [[CSS Modules]]
- [[Styled Components]]
- [[Performance Optimization]]
- [[Dark Mode]]
- [[React Project Structure]]
```