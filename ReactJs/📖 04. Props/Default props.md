---
title: What are defaultProps in React?
tags:
  - react
  - props
  - defaultprops
  - interview
  - react19
created: 2026-08-02
---

# What are defaultProps in React?

> [!INFO]
> **defaultProps** were used to provide default values for props when a parent component did not pass a value.

> [!WARNING]
> **For modern React (React 19+), `defaultProps` are no longer supported for Function Components.**
>
> Instead, use **JavaScript default parameters**. `defaultProps` are still supported for **Class Components**. :contentReference[oaicite:0]{index=0}

---

# Beginner Explanation

Imagine you order a pizza.

You don't choose a drink.

The restaurant automatically gives you:

```
Water
```

This is called a **default value**.

Similarly,

if a parent doesn't send a prop,

React can use a default value.

---

# Why Do We Need Default Values?

Suppose

```jsx
<User />
```

No name is passed.

Instead of displaying

```
Hello
```

or

```
Hello undefined
```

we want

```
Hello Guest
```

---

# Old Way (Legacy)

```jsx
function Greeting(props) {

    return (

        <h1>

            Hello {props.name}

        </h1>

    );

}

Greeting.defaultProps = {

    name: "Guest"

};

export default Greeting;
```

Usage

```jsx
<Greeting />
```

Output

```
Hello Guest
```

---

# Modern React (Recommended)

Use JavaScript default parameters.

```jsx
function Greeting({

    name = "Guest"

}) {

    return (

        <h1>

            Hello {name}

        </h1>

    );

}

export default Greeting;
```

Usage

```jsx
<Greeting />
```

Output

```
Hello Guest
```

This is the recommended approach for Function Components in React 19+. :contentReference[oaicite:1]{index=1}

---

# Passing a Value

```jsx
<Greeting

    name="Sanket"

/>
```

Output

```
Hello Sanket
```

The default value is ignored because a prop was provided.

---

# Multiple Default Values

```jsx
function User({

    name = "Guest",

    age = 18,

    city = "Pune"

}) {

    return (

        <>

            <h2>{name}</h2>

            <p>{age}</p>

            <p>{city}</p>

        </>

    );

}
```

---

# Class Component (Still Supported)

```jsx
import React, { Component } from "react";

class Greeting extends Component {

    static defaultProps = {

        name: "Guest"

    };

    render() {

        return (

            <h1>

                Hello {this.props.name}

            </h1>

        );

    }

}

export default Greeting;
```

`defaultProps` continue to work for Class Components. :contentReference[oaicite:2]{index=2}

---

# How It Works

```
Parent

↓

<Greeting />

↓

No "name" Prop

↓

Default Value Used

↓

Hello Guest
```

If

```
<Greeting name="John"/>
```

then

```
Hello John
```

---

# Internal Working

```
Component Render

↓

Check Prop

↓

Is Prop Missing?

↓

Yes

↓

Use Default Value

↓

Render UI
```

---

# Real-World Example

## Product Card

```jsx
function Product({

    image = "/default.png",

    title = "Unknown Product",

    price = 0

}) {

    return (

        <div>

            <img src={image} />

            <h2>{title}</h2>

            <h3>₹{price}</h3>

        </div>

    );

}
```

Usage

```jsx
<Product />
```

Output

```
Unknown Product

₹0

Default Image
```

---

# Common Mistakes

## Using `defaultProps` in Function Components

```jsx
Greeting.defaultProps = {

    name: "Guest"

};
```

This is legacy and should not be used in new Function Components. Use default parameters instead. :contentReference[oaicite:3]{index=3}

---

## Forgetting Default Values

```jsx
function User({

    name

}) {

    return (

        <h1>

            {name}

        </h1>

    );

}
```

Usage

```jsx
<User />
```

Output

```
undefined
```

Better

```jsx
function User({

    name = "Guest"

}) {

    return (

        <h1>

            {name}

        </h1>

    );

}
```

---

# Best Practices

- Use JavaScript default parameters in Function Components.
- Use meaningful default values.
- Keep defaults simple.
- Avoid using `defaultProps` for new Function Components.
- Use `static defaultProps` only in legacy Class Components when needed.

---

# Interview Questions

## Beginner

### What are defaultProps?

Default values used when a prop is not provided.

---

### Why are default values useful?

They prevent `undefined` values and make components more reliable.

---

### What is the modern way to define default values?

Using JavaScript default parameters.

```jsx
function User({

    name = "Guest"

}) {

    return <h1>{name}</h1>;

}
```

---

# Intermediate

### What happens if the parent passes a value?

The passed value overrides the default.

Example

```jsx
<User

    name="Alex"

/>
```

Output

```
Hello Alex
```

---

### What happens if the parent passes `undefined`?

The default value is used.

```jsx
<User

    name={undefined}

/>
```

Output

```
Guest
```

---

### What happens if the parent passes `null`?

`null` is considered an explicit value.

The default is **not** used.

```jsx
<User

    name={null}

/>
```

Output

```
null
```

(or whatever your component renders for `null`).

---

# Senior-Level Interview Questions

### Why were `defaultProps` removed for Function Components?

JavaScript already provides default parameters, making a separate React API unnecessary. Default parameters are simpler, work naturally with modern JavaScript and TypeScript, and reduce framework-specific APIs. :contentReference[oaicite:4]{index=4}

---

### Are `defaultProps` completely removed?

- **Function Components:** ❌ No longer recommended and removed in React 19.
- **Class Components:** ✅ Still supported. :contentReference[oaicite:5]{index=5}

---

### Which is better?

```jsx
Greeting.defaultProps
```

or

```jsx
function Greeting({

    name = "Guest"

})
```

**Answer**

Use default parameters for Function Components.

---

# Scenario-Based Questions

## Scenario 1

Your API sometimes doesn't return a user name.

How would you prevent

```
Hello undefined
```

from appearing?

**Answer**

Use a default parameter.

```jsx
function User({

    name = "Guest"

}) {

    return <h1>{name}</h1>;

}
```

---

## Scenario 2

An old React project uses

```jsx
Component.defaultProps
```

Should you keep it?

**Answer**

If it's a Class Component, it can remain.

If it's a Function Component, migrate to default parameters.

---

## Scenario 3

An interviewer asks:

"Why did React remove `defaultProps` from Function Components?"

**Expected Answer**

Because JavaScript default parameters provide the same functionality with simpler syntax and better integration with modern JavaScript and TypeScript.

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Purpose | Provide default prop values |
| Modern Function Components | Default parameters |
| Class Components | `static defaultProps` |
| Missing Prop | Default value used |
| Passed Value | Overrides default |
| Passed `undefined` | Default used |
| Passed `null` | Default not used |

---

# Related Notes

- [[Props]]