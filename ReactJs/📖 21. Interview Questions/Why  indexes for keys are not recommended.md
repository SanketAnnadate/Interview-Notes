---
title: Why Using Array Index as a Key is Not Recommended?
tags:
  - react
  - keys
  - reconciliation
  - virtual-dom
  - performance
  - interview
created: 2026-08-02
---

# Why Using Array Index as a Key is Not Recommended?

> [!WARNING]
> **Array indexes should generally NOT be used as React keys for dynamic lists.**
>
> React expects keys to be **unique**, **stable**, and **predictable**.
>
> When indexes are used as keys and the list changes (insert, delete, reorder), React may associate the wrong component with the wrong data.

---

# Beginner Explanation

Imagine a classroom.

Students are identified only by their seat numbers.

```
Seat 0 → John

Seat 1 → Sam

Seat 2 → Alex
```

Now John leaves.

```
Seat 0 → Sam

Seat 1 → Alex
```

The teacher thinks

```
John became Sam

Sam became Alex
```

Instead of realizing that **John left**.

This is exactly what happens when React uses array indexes as keys.

---

# Why React Needs Stable Keys

React compares

```
Old Virtual DOM

↓

New Virtual DOM
```

It matches elements using

```
key
```

If the key changes,

React assumes

```
Old Component Removed

↓

New Component Created
```

Instead of

```
Same Component

↓

Just Updated
```

---

# Example Using Index

```jsx
const users = [

    "John",

    "Sam",

    "Alex"

];

return (

    <ul>

        {

            users.map(

                (

                    user,

                    index

                ) =>

                <li

                    key={index}

                >

                    {user}

                </li>

            )

        }

    </ul>

);
```

Current keys

```text
0 John

1 Sam

2 Alex
```

---

# User Deletes John

New list

```text
Sam

Alex
```

React sees

```text
0 Sam

1 Alex
```

Old

```text
0 John

1 Sam

2 Alex
```

React thinks

```
John

↓

Sam

↓

Alex
```

Everything shifted.

---

# Correct Way

Each item has an ID.

```jsx
const users = [

    {

        id:101,

        name:"John"

    },

    {

        id:102,

        name:"Sam"

    },

    {

        id:103,

        name:"Alex"

    }

];
```

Render

```jsx
{

users.map(

user=>

<li

key={user.id}

>

{user.name}

</li>

)

}
```

Now

```
101 John

102 Sam

103 Alex
```

Delete John

```
102 Sam

103 Alex
```

React immediately knows

```
Only 101 was removed.
```

---

# Visual Comparison

## Using Index

```text
Old

0 John

1 Sam

2 Alex

↓

Delete John

↓

New

0 Sam

1 Alex
```

React compares

```text
0 John → Sam

1 Sam → Alex
```

Wrong identity.

---

## Using ID

```text
Old

101 John

102 Sam

103 Alex

↓

Delete John

↓

102 Sam

103 Alex
```

React compares

```text
101 Removed

102 Same

103 Same
```

Correct identity.

---

# Internal Working

## With Index

```text
Render

↓

Index Changes

↓

React Thinks

Everything Changed

↓

Re-render Components

↓

Possible State Bugs
```

---

## With Stable IDs

```text
Render

↓

Keys Compared

↓

Only Changed Items

↓

Minimal DOM Updates
```

---

# Problem 1 — Wrong Component State

Imagine

```jsx
<TodoItem

key={index}

/>
```

Each Todo contains

```jsx
const [

text,

setText

] = useState("");
```

Current list

```text
0 Buy Milk

1 Read Book

2 Exercise
```

User types

```text
Read Book

↓

React State

"I'll read later"
```

Now delete

```
Buy Milk
```

Indexes become

```text
0 Read Book

1 Exercise
```

React moves the old state.

Now

```
Exercise

↓

"I'll read later"
```

Wrong component.

---

# Problem 2 — Input Fields

Imagine

```jsx
<input />
```

inside a list.

User types

```
John
```

Delete first item.

Input suddenly moves to another row.

This is a classic interview question.

Reason

```
Index Keys
```

---

# Problem 3 — Animations

Libraries like

- Framer Motion
- React Transition Group

depend on stable keys.

Changing indexes

↓

Broken animations.

---

# Problem 4 — Performance

Without stable keys

React destroys

```
Component A

Component B

Component C
```

and recreates them.

Instead of

```
Keeping B

Keeping C

Removing A
```

More work.

Lower performance.

---

# When is Index Safe?

Only when

✅ List never changes

✅ No sorting

✅ No filtering

✅ No insertion

✅ No deletion

Example

```jsx
const weekdays = [

"Monday",

"Tuesday",

"Wednesday"

];
```

Static list.

Index is acceptable.

---

# Bad Examples

```jsx
key={index}
```

```jsx
key={Math.random()}
```

```jsx
key={Date.now()}
```

```jsx
key={uuid()}
```

inside render.

All are unstable if regenerated every render.

---

# Good Examples

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
key={uuidFromDatabase}
```

Stable identifiers.

---

# Performance Comparison

| Using Index | Using Stable ID |
|--------------|----------------|
| Wrong identity | Correct identity |
| More re-renders | Minimal re-renders |
| State mismatch | State preserved |
| Broken animations | Stable animations |
| Bad for dynamic lists | Best practice |

---

# Common Mistakes

## Using Index for Todo List

Wrong

```jsx
todos.map(

(

todo,

index

)=>

<Todo

key={index}

/>

)
```

---

Correct

```jsx
todos.map(

todo=>

<Todo

key={todo.id}

/>

)
```

---

## Generating Keys on Every Render

Wrong

```jsx
key={Math.random()}
```

Every render

↓

New key

↓

Component remounts.

---

# Best Practices

✅ Use database IDs.

✅ Keep keys stable.

✅ Never use random values.

✅ Avoid indexes in dynamic lists.

✅ Use indexes only for fixed, static lists.

---

# Interview Questions

## Beginner

### Why do we need keys?

To uniquely identify list items during rendering.

---

### Why shouldn't we use indexes?

Indexes change when the list changes.

---

### Which key is best?

Database ID.

---

# Intermediate

### What happens when the first item is deleted?

All following indexes shift.

React may reuse components incorrectly, causing UI and state issues.

---

### When can index be used?

Only for static lists that never change.

---

### Why is `Math.random()` worse than index?

Because it changes every render, forcing React to unmount and remount every component.

---

# Senior-Level Interview Questions

### Explain why indexes break React's reconciliation.

React uses keys to match elements between the previous and next Virtual DOM trees.

Indexes change when elements are inserted, deleted, or reordered.

This causes React to associate existing component instances with different data, leading to incorrect state preservation and unnecessary DOM work.

---

### What problems occur with forms?

Input values, cursor positions, focus state, and local component state can appear to "move" to the wrong row after list modifications.

---

### Why do stable keys improve performance?

Stable keys let React preserve component instances, minimize DOM updates, retain local state, and perform reconciliation more efficiently.

---

# Scenario-Based Questions

## Scenario 1

A Todo application uses

```jsx
key={index}
```

After deleting the first task, text typed into another task appears on the wrong item.

Why?

**Answer**

React reused component instances because the indexes shifted. Stable IDs would preserve the correct association between data and component state.

---

## Scenario 2

An interviewer asks:

**"Is using index always wrong?"**

**Answer**

No.

It's acceptable for static lists whose order and contents never change.

For dynamic lists, use stable unique IDs.

---

## Scenario 3

Your backend returns

```json
[
    {
        "id": 45,
        "name": "Laptop"
    }
]
```

Which key should you use?

```jsx
key={product.id}
```

---

# Quick Revision

| Question | Answer |
|----------|--------|
| Why avoid index? | Index changes when list changes |
| Best key | Stable unique ID |
| Dynamic list | Never use index |
| Static list | Index is acceptable |
| Helps | Reconciliation, state preservation, performance |

---

# Excalidraw Diagram

```text
                Dynamic List

                     │

             Delete First Item

                     │

      ┌──────────────┴──────────────┐

      │                             │

   Using Index                  Using ID

      │                             │

 Indexes Shift                 IDs Stay Same

      │                             │

 Wrong Component             Correct Component

 Identity                    Identity

      │                             │

 State Moves                State Preserved

      │                             │

 More DOM Updates           Minimal DOM Updates

      └──────────────┬──────────────┘

                     │

          Fast & Correct Reconciliation
```

---

# Related Notes

- [[React Interview Roadmap]]
```