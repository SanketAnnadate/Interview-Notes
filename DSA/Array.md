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
# 📘 Array Algorithms Roadmap

## ⭐⭐⭐⭐⭐ Foundation (Must Master)

### 1. Traversal
- Forward Traversal
- Reverse Traversal
- Frequency Count
- Min / Max
- Sum / Average

---

### 2. Searching

- Linear Search
- Binary Search (Sorted Array)

---

### 3. Sorting

- Bubble Sort
- Selection Sort
- Insertion Sort
- Merge Sort
- Quick Sort
- Heap Sort

---

### 4. Prefix Sum

- Prefix Sum Array
- Range Sum Query
- Equilibrium Index
- Difference Array

---

### 5. Kadane's Algorithm

- Maximum Subarray Sum
- Circular Maximum Subarray

---

## ⭐⭐⭐⭐⭐ Interview Patterns

### 6. Two Pointers

- Pair Sum
- Remove Duplicates
- Move Zeroes
- Merge Sorted Arrays
- Container With Most Water
- 3Sum
- 4Sum

---

### 7. Sliding Window

#### Fixed Window

- Maximum Sum
- Average
- K-size Window

#### Variable Window

- Longest Substring
- Minimum Window
- Fruit Into Baskets

---

### 8. Binary Search on Arrays

- Search Insert Position
- Rotated Sorted Array
- Peak Element
- Search Matrix
- First & Last Position
- Binary Search on Answer

---

## ⭐⭐⭐⭐ Medium Priority

### 9. Hashing

- Two Sum
- Frequency Count
- Longest Consecutive Sequence
- Duplicate Detection
- Majority Element

---

### 10. Matrix Algorithms

- Matrix Traversal
- Spiral Matrix
- Rotate Matrix
- Set Matrix Zeroes
- Search in Matrix
- Diagonal Traversal

---

### 11. Interval Problems

- Merge Intervals
- Insert Interval
- Meeting Rooms
- Non-overlapping Intervals

---

### 12. Dutch National Flag

- Sort Colors
- Three-way Partition

---

### 13. Moore Voting Algorithm

- Majority Element
- Majority Element II

---

## ⭐⭐⭐ Medium Priority

### 14. Cyclic Sort

- Missing Number
- Find Duplicate
- First Missing Positive
- Set Mismatch

---

### 15. XOR Algorithms

- Single Number
- Missing Number
- Two Unique Numbers
- XOR Prefix

---

### 16. Monotonic Stack (Array Problems)

- Next Greater Element
- Previous Greater Element
- Daily Temperatures
- Largest Rectangle
- Trapping Rain Water

---

## ⭐⭐ Advanced

### 17. Divide & Conquer

- Merge Sort
- Count Inversions
- Reverse Pairs

---

### 18. Quick Select

- Kth Largest
- K Closest Elements

---

### 19. Heap Based

- Top K Frequent
- K Largest
- Merge K Sorted Arrays

---

### 20. Advanced Prefix Techniques

- Prefix XOR
- Prefix Product
- Difference Array
- Sweep Line

---

# 🎯 Google / FAANG Priority

⭐⭐⭐⭐⭐ Must Master

- Traversal
- Searching
- Sorting
- Prefix Sum
- Kadane's Algorithm
- Two Pointers
- Sliding Window
- Binary Search
- Hashing

---

⭐⭐⭐⭐ Learn Well

- Matrix
- Merge Intervals
- Dutch National Flag
- Moore Voting
- Cyclic Sort

---

⭐⭐⭐ Learn

- XOR
- Monotonic Stack
- Quick Select
- Heap

---

⭐⭐ Optional

- Difference Array
- Sweep Line
- Count Inversions

---

# 📊 Practice Target

| Topic | Problems |
|--------|---------:|
| Traversal | 10 |
| Searching | 10 |
| Sorting | 15 |
| Prefix Sum | 15 |
| Kadane | 10 |
| Two Pointers | 25 |
| Sliding Window | 30 |
| Binary Search | 30 |
| Hashing | 25 |
| Matrix | 20 |
| Intervals | 15 |
| Dutch National Flag | 5 |
| Moore Voting | 5 |
| Cyclic Sort | 10 |
| XOR | 10 |
| Monotonic Stack | 15 |
| Quick Select | 5 |
| Heap | 10 |

**Total Target:** ≈ **250 Array-related problems**
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


[[Array Solutions]]