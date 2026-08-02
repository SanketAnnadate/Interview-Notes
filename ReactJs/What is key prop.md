---
title: React Keys
tags:
  - react
  - keys
  - lists
  - reconciliation
  - virtual-dom
  - interview
created: 2026-08-02
---

# What are Keys in React?

> [!INFO]
> A **key** is a **unique identifier** that React uses to identify elements in a list during rendering.

> [!IMPORTANT]
> Keys help React determine:
>
> - Which element changed
> - Which element was added
> - Which element was removed
> - Which element stayed the same
>
> Keys make React's **Reconciliation Algorithm** fast and efficient.

---

# Beginner Explanation

Imagine a classroom.

Without roll numbers

```
Rahul

Amit

John

David
```

Teacher cannot easily identify students.

---

With roll numbers

```
1 Rahul

2 Amit

3 John

4 David
```

Now every student has a unique identity.

React works the same way.

Every item in a list should have a unique **key**.

---

# Why Do We Need Keys?

Suppose we render

```text
Apple

Banana

Orange
```

Now a new item is inserted.

```text
Apple

Mango

Banana

Orange
```

Without keys,

React thinks

```
Banana → Mango

Orange → Banana

New Orange
```

Everything changes.

---

With keys

```
1 Apple

4 Mango

2 Banana

3 Orange
```

React immediately understands

```
Only Mango was inserted.
```

No unnecessary updates.

---

# Without Keys

```jsx
function App() {

    const fruits = [

        "Apple",

        "Banana",

        "Orange"

    ];

    return (

        <ul>

            {

                fruits.map(

                    fruit =>

                    <li>

                        {fruit}

                    </li>

                )

            }

        </ul>

    );

}
```

React Warning

```
Each child in a list should have a unique "key" prop.
```

---

# With Keys

```jsx
function App() {

    const fruits = [

        {

            id:1,

            name:"Apple"

        },

        {

            id:2,

            name:"Banana"

        },

        {

            id:3,

            name:"Orange"

        }

    ];

    return (

        <ul>

            {

                fruits.map(

                    fruit =>

                    <li

                        key={fruit.id}

                    >

                        {fruit.name}

                    </li>

                )

            }

        </ul>

    );

}
```

---

# What Happens Internally?

Without Keys

```text
Old List

Apple

Banana

Orange

↓

New List

Apple

Mango

Banana

Orange

↓

React compares

Banana → Mango

Orange → Banana

New Orange

Many unnecessary updates
```

---

With Keys

```text
Old

1 Apple

2 Banana

3 Orange

↓

New

1 Apple

4 Mango

2 Banana

3 Orange

↓

React detects

Only key 4 added

Done.
```

---

# Internal Working

```text
Component Render

↓

Virtual DOM

↓

Keys Compared

↓

Reconciliation

↓

Only Changed Nodes Updated

↓

Real DOM Updated
```

---

# Why Are Keys Important?

Keys improve

- Performance
- Correct UI updates
- Component identity
- State preservation
- Reconciliation speed

---

# Real World Example

Suppose you have a Todo List.

```text
Task 1

Task 2

Task 3
```

User deletes

```
Task 2
```

Without keys

React may unnecessarily re-render Task 3.

With keys

Only Task 2 disappears.

Everything else remains unchanged.

---

# Keys and Component State

Imagine

```jsx
<Todo

    key={todo.id}

/>
```

Each Todo component has its own state.

When keys remain stable,

React preserves each component's state.

Changing keys forces React to create a new component instance.

---

# Why Not Use Index as Key?

Bad

```jsx
items.map(

    (

        item,

        index

    ) =>

    <Card

        key={index}

    />

)
```

Suppose

```
A

B

C
```

Delete

```
A
```

Indexes become

```
0 B

1 C
```

React thinks

```
A became B

B became C
```

State and UI can become incorrect.

---

# Correct

```jsx
items.map(

    item =>

    <Card

        key={item.id}

    />

)
```

Stable identity.

---

# When Can Index Be Used?

Only if

- List never changes.
- Items are never reordered.
- Items are never inserted.
- Items are never deleted.

Example

```jsx
const weekdays=[

"Mon",

"Tue",

"Wed"

];
```

Safe.

---

# Good Key Examples

```jsx
key={user.id}
```

```jsx
key={product.id}
```

```jsx
key={email}
```

```jsx
key={uuid}
```

---

# Bad Key Examples

```jsx
key={index}
```

```jsx
key={Math.random()}
```

```jsx
key={Date.now()}
```

These change between renders and cause unnecessary remounting.

---

# Key vs ID

| id | key |
|----|-----|
| Data property | React prop |
| Stored in database | Used by React internally |
| Available in component props | Not accessible via `props.key` |
| Business identity | Rendering identity |

---

# Does Child Component Receive key?

No.

Wrong

```jsx
function Card(

    props

){

    console.log(

        props.key

    );

}
```

Output

```
undefined
```

If the component needs the identifier,

pass it explicitly.

```jsx
<Card

    key={user.id}

    id={user.id}

/>
```

---

# Performance

Keys make React's **Diffing Algorithm** efficient.

Without keys

```
O(n²)
```

comparisons may be needed in complex reorder cases.

With stable keys

React can match elements directly and minimize DOM operations.

---

# Common Mistakes

## Using Random Keys

```jsx
key={Math.random()}
```

Every render creates a new key.

React destroys and recreates every component.

---

## Using Index for Dynamic Lists

Wrong

```jsx
key={index}
```

Correct

```jsx
key={item.id}
```

---

## Duplicate Keys

Wrong

```jsx
key={1}

key={1}
```

Keys must be unique among siblings.

---

# Best Practices

✅ Use unique database IDs.

✅ Keys should be stable.

✅ Never generate random keys during render.

✅ Avoid array index unless the list is static.

✅ Keep keys unique only among siblings (they don't need to be globally unique).

---

# Interview Questions

## Beginner

### What is a key in React?

A unique identifier used by React to identify list items during rendering.

---

### Why do we use keys?

To help React efficiently update, insert, remove, and reorder elements.

---

### Where are keys used?

While rendering collections using

```jsx
map()
```

---

### Are keys visible in the browser?

No.

They are used internally by React.

---

# Intermediate

### Why shouldn't we use array index as key?

Indexes change when items are inserted, deleted, or reordered, causing incorrect component identity and state issues.

---

### Can two sibling components have the same key?

No.

Keys must be unique among sibling elements.

---

### Does React pass key as a prop?

No.

Use another prop if the child needs the value.

---

# Senior-Level Interview Questions

### How do keys help the Reconciliation Algorithm?

React compares old and new Virtual DOM trees. Stable keys allow React to match corresponding elements directly, preserving component state and minimizing DOM operations.

---

### What happens if a key changes?

React treats it as a completely new component.

- Old component unmounts.
- New component mounts.
- Local state is reset.

---

### Why are stable keys important in forms?

Imagine a list of editable input fields.

If indexes are used as keys and the list is reordered, users may suddenly see input values appear in the wrong rows because component state has been reassigned.

Stable keys prevent this issue.

---

# Scenario-Based Questions

## Scenario 1

An interviewer asks:

**"Why is using `Math.random()` as a key a bad idea?"**

**Answer**

Because the key changes on every render. React unmounts and recreates every component, losing state and hurting performance.

---

## Scenario 2

A user edits the second item in a todo list. Another item is inserted at the top.

Why does the edited value suddenly appear in the wrong row?

**Answer**

The list is using array indexes as keys. Component identity changed after insertion.

---

## Scenario 3

A backend returns

```json
[
    {
        "id":101,
        "name":"Laptop"
    }
]
```

Which key should you use?

```jsx
key={product.id}
```

---

# Quick Revision

| Topic | Summary |
|--------|---------|
| Key | Unique identifier for list items |
| Purpose | Help React identify elements |
| Used With | `map()` and dynamic lists |
| Best Key | Database ID / UUID |
| Avoid | `index`, `Math.random()`, `Date.now()` |
| Visible in Props | ❌ No |
| Helps | Reconciliation, performance, state preservation |

---

# Excalidraw Diagram

```text
                    List Rendering

                          │

                    items.map()

                          │

            ┌─────────────┴─────────────┐

            │                           │

       Without Keys                With Stable Keys

            │                           │

   Compare by Position         Compare by Identity

            │                           │

 More DOM Updates            Minimal DOM Updates

            │                           │

    Possible State Bugs      State Preserved

            └─────────────┬─────────────┘

                          │

                 Faster Reconciliation
```

---

# Related Notes

- 
```