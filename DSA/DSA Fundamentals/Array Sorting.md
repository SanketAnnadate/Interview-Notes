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

