---
tags:
  - dsa
  - sorting
  - bubble-sort
priority: ⭐⭐
difficulty: Easy
status: Completed
---

# 📘 Bubble Sort (Optimized)

> Bubble Sort repeatedly compares **adjacent elements** and swaps them if they are in the wrong order. After each pass, the **largest unsorted element moves to its correct position**.

---

# ✅ Java Implementation

```java
public static void bubbleSort(int[] arr) {

    int n = arr.length;

    for (int i = 0; i < n - 1; i++) {

        boolean swapped = false;

        for (int j = 0; j < n - i - 1; j++) {

            if (arr[j] > arr[j + 1]) {

                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;

                swapped = true;
            }
        }

        // Stop if the array is already sorted
        if (!swapped) {
            break;
        }
    }
}
```

---

# 🔍 Code Explanation

## Step 1: Get Array Length

```java
int n = arr.length;
```

Stores the size of the array.

Example

```
Array

5 1 4 2 8

↓

n = 5
```

---

## Step 2: Outer Loop

```java
for (int i = 0; i < n - 1; i++)
```

**Purpose**

- Controls the number of passes.
- After every pass, one largest element reaches its correct position.

Example

```
Pass 1

5 1 4 2 8

↓

1 4 2 5 8
```

Largest element

```
8
```

is fixed.

Maximum Passes

```
n - 1
```

---

## Step 3: Swapped Flag

```java
boolean swapped = false;
```

**Purpose**

Checks whether any swap happened during the current pass.

If no swap occurs,

```
Array is already sorted.
```

Algorithm stops early.

---

## Step 4: Inner Loop

```java
for (int j = 0; j < n - i - 1; j++)
```

**Purpose**

Compare adjacent elements.

```
arr[j]

↓

arr[j+1]
```

Example

```
5 1

↓

Swap
```

---

### Why `n - i - 1`?

After every pass,

one largest element is already fixed.

```
Pass 1

1 4 2 5 8

↑
No need to compare 8 again.
```

Therefore,

```
Inner Loop

↓

Becomes smaller every pass.
```

---

## Step 5: Compare

```java
if (arr[j] > arr[j + 1])
```

If left element is greater than right element,

swap them.

Example

```
5 2

↓

Swap

↓

2 5
```

---

## Step 6: Swap

```java
int temp = arr[j];
arr[j] = arr[j + 1];
arr[j + 1] = temp;
```

Temporary variable stores one value while swapping.

Before

```
5 2
```

After

```
2 5
```

---

## Step 7: Mark Swap

```java
swapped = true;
```

Indicates that at least one swap happened.

---

## Step 8: Early Exit

```java
if (!swapped) {
    break;
}
```

If no swaps occurred,

the array is already sorted.

Stop immediately.

---

# 📊 Dry Run

Input

```
5 1 4 2 8
```

### Pass 1

```
5 1

↓

1 5 4 2 8
```

```
5 4

↓

1 4 5 2 8
```

```
5 2

↓

1 4 2 5 8
```

```
5 8

↓

No Swap
```

Result

```
1 4 2 5 8
```

---

### Pass 2

```
4 2

↓

Swap
```

Result

```
1 2 4 5 8
```

---

### Pass 3

No swaps.

Algorithm stops.

---

# ⏱️ Time Complexity

| Case | Complexity | Reason |
|------|------------|--------|
| Best Case | **O(n)** | Already sorted, one pass only |
| Average Case | **O(n²)** | Randomly ordered elements |
| Worst Case | **O(n²)** | Reverse sorted array |

---

# 💾 Space Complexity

| Complexity | Reason |
|------------|--------|
| **O(1)** | Uses only one temporary variable (`temp`) and one boolean (`swapped`) |

---

# 📈 Number of Comparisons

| Pass | Comparisons |
|------|------------:|
| Pass 1 | n - 1 |
| Pass 2 | n - 2 |
| Pass 3 | n - 3 |
| ... | ... |
| Last Pass | 1 |

Total Comparisons

```
(n-1) + (n-2) + ... + 1

=

n(n-1)/2

=

O(n²)
```

---

# 🔄 Number of Swaps

| Case | Swaps |
|------|------:|
| Best | 0 |
| Average | Approximately n²/2 |
| Worst | n(n-1)/2 |

---

# 📋 Properties

| Property | Value |
|----------|-------|
| Algorithm Type | Comparison Sort |
| Technique | Adjacent Swapping |
| Stable | ✅ Yes |
| In-place | ✅ Yes |
| Adaptive | ✅ Yes (Optimized Version) |
| Recursive | ❌ No |
| Online Algorithm | ❌ No |

---

# 👍 Advantages

- Easy to understand
- Easy to implement
- Stable
- In-place
- Adaptive (Optimized Version)
- Good for educational purposes

---

# 👎 Disadvantages

- Slow for large datasets
- High number of comparisons
- High number of swaps
- Rarely used in production

---

# 💼 Interview Questions

| Question | Answer |
|----------|--------|
| Why use `swapped`? | Stops early if the array is already sorted. |
| Why `j < n - i - 1`? | Last `i` elements are already sorted. |
| Why is Bubble Sort stable? | Equal elements never change their relative order. |
| Why is Bubble Sort in-place? | Only constant extra memory is used. |
| What is the Best Case? | O(n) |
| What is the Worst Case? | O(n²) |
| Space Complexity? | O(1) |

---

# 🧠 Memory Trick

```
Compare

↓

Swap

↓

Largest Goes Right

↓

Repeat
```

Remember:

> **Each pass places the largest remaining element at its correct position.**

---

# 📝 Revision Checklist

- [ ] Understand adjacent swapping
- [ ] Know why `n - i - 1` is used
- [ ] Explain `swapped` optimization
- [ ] Dry run manually
- [ ] Write code without looking
- [ ] Explain Time Complexity
- [ ] Explain Space Complexity
- [ ] Explain Stable vs In-place
- [ ] Solve Bubble Sort interview questions

---

# 🔗 Related Notes

- [[Sorting]]
- [[Selection Sort]]
- [[Insertion Sort]]
- [[Merge Sort]]
- [[Quick Sort]]
- [[Heap Sort]]
- [[Arrays]]
- [[Time Complexity]]