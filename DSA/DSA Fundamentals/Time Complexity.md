---
tags: [dsa, fundamentals, complexity]
status: Completed
difficulty: Easy
---

# 📘 Time Complexity

> Time Complexity measures **how the running time of an algorithm grows as the input size (n) increases**. It does **not** measure the actual execution time in seconds.

---

# 🎯 Learning Objectives

After completing this topic, you should be able to:

- Understand Big O notation.
- Analyze loops and recursive functions.
- Compare different algorithms.
- Optimize inefficient solutions.
- Calculate the time complexity of interview questions.

---

# Why Time Complexity?

Consider searching for an element.

### Linear Search

```java
for(int i = 0; i < n; i++){
    if(arr[i] == target)
        return i;
}
```

Time Complexity:

```
O(n)
```

---

### Binary Search

```java
while(low <= high){
    int mid = (low + high) / 2;

    if(arr[mid] == target)
        return mid;

    if(arr[mid] < target)
        low = mid + 1;
    else
        high = mid - 1;
}
```

Time Complexity

```
O(log n)
```

As **n increases**, Binary Search becomes much faster than Linear Search.

---

# Definition

Time Complexity is the number of operations an algorithm performs as a function of the input size **n**.

It is represented using **Big O Notation**.

---

# Types of Complexity

## Best Case

Minimum number of operations.

Example:

Searching the first element.

```
O(1)
```

---

## Average Case

Average number of operations over all possible inputs.

---

## Worst Case

Maximum number of operations.

Example:

Searching the last element using Linear Search.

```
O(n)
```

Interviewers generally expect the **worst-case time complexity** unless stated otherwise.

---

# Asymptotic Notations

## 1. Big O (Upper Bound)

Represents the **maximum running time**.

Example:

```
Linear Search → O(n)
```

---

## 2. Omega (Ω)

Represents the **minimum running time**.

Example:

```
Linear Search → Ω(1)
```

---

## 3. Theta (Θ)

Represents the **exact running time**.

Example:

```
Merge Sort → Θ(n log n)
```

---

# Common Time Complexities

| Complexity | Name | Example |
|------------|------|---------|
| O(1) | Constant | Array Access |
| O(log n) | Logarithmic | Binary Search |
| O(√n) | Square Root | Prime Checking |
| O(n) | Linear | Linear Search |
| O(n log n) | Linearithmic | Merge Sort, Heap Sort |
| O(n²) | Quadratic | Bubble Sort |
| O(n³) | Cubic | Floyd-Warshall |
| O(2ⁿ) | Exponential | Subsets |
| O(n!) | Factorial | Permutations |

---

# Complexity Growth Order

Fastest → Slowest

```
O(1)

↓

O(log n)

↓

O(√n)

↓

O(n)

↓

O(n log n)

↓

O(n²)

↓

O(n³)

↓

O(2ⁿ)

↓

O(n!)
```

---

# Time Complexity of Loops

## Single Loop

```java
for(int i = 0; i < n; i++)
```

```
O(n)
```

---

## Nested Loop

```java
for(int i = 0; i < n; i++){
    for(int j = 0; j < n; j++){
    }
}
```

```
O(n²)
```

---

## Triple Nested Loop

```java
for(...)
    for(...)
        for(...)
```

```
O(n³)
```

---

## Loop Dividing by 2

```java
while(n > 1){
    n /= 2;
}
```

```
O(log n)
```

---

## Loop Doubling

```java
for(int i = 1; i < n; i *= 2)
```

```
O(log n)
```

---

## Loop Inside Logarithmic Loop

```java
for(int i = 1; i < n; i *= 2){
    for(int j = 0; j < n; j++){
    }
}
```

```
O(n log n)
```

---

# Time Complexity Rules

## Rule 1: Drop Constants

```
O(2n)

↓

O(n)
```

```
O(1000n)

↓

O(n)
```

---

## Rule 2: Keep Highest Order Term

```
O(n² + n)

↓

O(n²)
```

```
O(n³ + n² + n)

↓

O(n³)
```

---

## Rule 3: Sequential Statements Add

```java
for(...)
for(...)
```

```
O(n) + O(n)

=

O(2n)

=

O(n)
```

---

## Rule 4: Nested Statements Multiply

```java
for(...)
    for(...)
```

```
O(n × n)

=

O(n²)
```

---

# Recursion Complexity

## Linear Recursion

```java
fun(n-1)
```

```
O(n)
```

---

## Binary Recursion

```java
fun(n-1)

fun(n-1)
```

```
O(2ⁿ)
```

---

## Binary Search Recursion

```java
fun(n/2)
```

```
O(log n)
```

---

# Common Algorithm Complexities

| Algorithm | Time |
|------------|------|
| Linear Search | O(n) |
| Binary Search | O(log n) |
| Bubble Sort | O(n²) |
| Selection Sort | O(n²) |
| Insertion Sort | O(n²) |
| Merge Sort | O(n log n) |
| Quick Sort (Average) | O(n log n) |
| Quick Sort (Worst) | O(n²) |
| Heap Sort | O(n log n) |
| BFS | O(V + E) |
| DFS | O(V + E) |
| Dijkstra | O((V + E) log V) |
| Union Find | Nearly O(1) (Amortized) |

---

# How to Calculate Time Complexity

### Step 1

Count the loops.

### Step 2

Check if loops are nested.

### Step 3

Check if loop increments normally or exponentially.

### Step 4

Ignore constants.

### Step 5

Keep the highest order term.

---

# Examples

### Example 1

```java
for(int i = 0; i < n; i++)
```

```
O(n)
```

---

### Example 2

```java
for(int i = 0; i < n; i++)
    for(int j = 0; j < n; j++)
```

```
O(n²)
```

---

### Example 3

```java
for(int i = 1; i < n; i *= 2)
```

```
O(log n)
```

---

### Example 4

```java
for(int i = 1; i < n; i *= 2)
    for(int j = 0; j < n; j++)
```

```
O(n log n)
```

---

# Common Interview Mistakes

❌ Forgetting nested loops multiply.

❌ Forgetting sequential loops add.

❌ Including constants.

❌ Ignoring recursion.

❌ Confusing Time Complexity with Execution Time.

❌ Forgetting average vs worst case.

---

# Interview Tips

✅ Always mention:

- Brute Force Complexity
- Optimal Complexity

✅ Explain **why** your solution is faster.

✅ Compare your approach with the previous one.

Example:

```
Brute Force

O(n²)

↓

Optimized

O(n log n)
```

---

# Revision Cheat Sheet

| Pattern | Complexity |
|----------|------------|
| Array Access | O(1) |
| Linear Search | O(n) |
| Binary Search | O(log n) |
| Single Loop | O(n) |
| Nested Loop | O(n²) |
| Divide by 2 | O(log n) |
| Merge Sort | O(n log n) |
| Quick Sort (Average) | O(n log n) |
| Bubble Sort | O(n²) |
| BFS / DFS | O(V + E) |
| Subsets | O(2ⁿ) |
| Permutations | O(n!) |

---

# Memory Trick

```
Excellent

O(1)

↓

O(log n)

↓

Good

O(n)

↓

Acceptable

O(n log n)

↓

Slow

O(n²)

↓

Very Slow

O(n³)

↓

Avoid if Possible

O(2ⁿ)

↓

Worst

O(n!)
```

---

# Related Notes

