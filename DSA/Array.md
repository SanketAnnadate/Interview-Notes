# 📘 Arrays - Quick Revision Notes

## Definition
- Collection of elements of the **same data type**.
- Stored in **contiguous memory locations**.
- Supports **random access** using index.

---

## Properties

- Fixed Size
- Zero-based Indexing
- Continuous Memory
- Same Data Type
- O(1) Random Access

---

## Declaration

```java
int[] arr = new int[5];
```

```java
int[] arr = {1,2,3,4,5};
```

---

## Traversal

```java
for(int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

Enhanced for loop

```java
for(int num : arr) {
    System.out.println(num);
}
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Access | O(1) |
| Update | O(1) |
| Traversal | O(n) |
| Search (Linear) | O(n) |
| Search (Binary) | O(log n) |
| Insert | O(n) |
| Delete | O(n) |

---

## Advantages

- Fast Random Access
- Simple Implementation
- Cache Friendly
- Less Memory Overhead

---

## Disadvantages

- Fixed Size
- Costly Insert/Delete
- Same Data Type Only
- Memory Wastage (if oversized)

---

## Java Array APIs

```java
Arrays.sort(arr);
Arrays.binarySearch(arr, key);
Arrays.fill(arr, value);
Arrays.copyOf(arr, size);
Arrays.equals(arr1, arr2);
Arrays.toString(arr);
```

Import

```java
import java.util.Arrays;
```

---

## Common Interview Patterns

- Traversal
- Prefix Sum
- Sliding Window
- Two Pointers
- Binary Search
- Sorting
- Hashing
- Matrix Traversal

---

## Important Algorithms

- Linear Search
- Binary Search
- Prefix Sum
- Kadane's Algorithm
- Dutch National Flag
- Moore Voting Algorithm
- Merge Sorted Arrays
- Rotate Array

---

## Frequently Asked Problems

### Easy

- Two Sum
- Maximum Subarray
- Move Zeroes
- Missing Number
- Best Time to Buy Stock
- Remove Duplicates

### Medium

- Rotate Array
- Product of Array Except Self
- Merge Intervals
- Set Matrix Zeroes
- Spiral Matrix

### Hard

- Trapping Rain Water
- Median of Two Sorted Arrays
- First Missing Positive
- Count Inversions

---

## Common Mistakes

- ArrayIndexOutOfBoundsException
- Off-by-One Errors
- Forgetting `arr.length`
- Integer Overflow
- Modifying Array During Traversal
- Not Handling Empty Arrays

---

## Interview Tips

✅ Explain Brute Force first.

✅ Optimize step by step.

✅ Mention Time & Space Complexity.

✅ Handle Edge Cases:
- Empty Array
- Single Element
- Duplicate Values
- Negative Numbers
- Sorted Array

---

## Revision Checklist

- [ ] Declaration & Initialization
- [ ] Traversal
- [ ] Search
- [ ] Insert/Delete
- [ ] Time Complexity
- [ ] Java Array APIs
- [ ] Prefix Sum
- [ ] Sliding Window
- [ ] Two Pointers
- [ ] Binary Search
- [ ] Kadane's Algorithm
- [ ] Moore Voting