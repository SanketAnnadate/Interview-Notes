# 📘 Array Traversal Examples

Given Array

```java
int[] arr = {10, 20, 30, 40, 50};
```

---

# 1. Forward Traversal ⭐⭐⭐⭐⭐

Visit elements from left to right.

### Example

```java
for (int i = 0; i < arr.length; i++) {
    System.out.print(arr[i] + " ");
}
```

### Output

```
10 20 30 40 50
```

### Time Complexity

```
O(n)
```

### Uses

- Printing elements
- Searching
- Counting
- Summation

---

# 2. Reverse Traversal ⭐⭐⭐⭐⭐

Visit elements from right to left.

### Example

```java
for (int i = arr.length - 1; i >= 0; i--) {
    System.out.print(arr[i] + " ");
}
```

### Output

```
50 40 30 20 10
```

### Time Complexity

```
O(n)
```

### Uses

- Reverse printing
- Reverse processing
- Stack-like problems

---

# 3. Frequency Count ⭐⭐⭐⭐⭐

Count how many times each element appears.

### Example

```java
int[] arr = {1,2,2,3,3,3};

HashMap<Integer, Integer> map = new HashMap<>();

for (int num : arr) {
    map.put(num, map.getOrDefault(num, 0) + 1);
}

System.out.println(map);
```

### Output

```
1 -> 1
2 -> 2
3 -> 3
```

### Time Complexity

```
O(n)
```

### Uses

- Majority Element
- Duplicate Detection
- Character Frequency
- Hashing Problems

---

# 4. Find Minimum ⭐⭐⭐⭐⭐

### Example

```java
int min = arr[0];

for (int num : arr) {
    if (num < min)
        min = num;
}

System.out.println(min);
```

### Output

```
10
```

### Time Complexity

```
O(n)
```

---

# 5. Find Maximum ⭐⭐⭐⭐⭐

### Example

```java
int max = arr[0];

for (int num : arr) {
    if (num > max)
        max = num;
}

System.out.println(max);
```

### Output

```
50
```

### Time Complexity

```
O(n)
```

---

# 6. Find Sum ⭐⭐⭐⭐⭐

### Example

```java
int sum = 0;

for (int num : arr) {
    sum += num;
}

System.out.println(sum);
```

### Output

```
150
```

### Time Complexity

```
O(n)
```

---

# 7. Find Average ⭐⭐⭐⭐

### Example

```java
int sum = 0;

for (int num : arr) {
    sum += num;
}

double average = (double) sum / arr.length;

System.out.println(average);
```

### Output

```
30.0
```

### Time Complexity

```
O(n)
```

---

# 8. Count Even & Odd ⭐⭐⭐⭐

### Example

```java
int even = 0;
int odd = 0;

for (int num : arr) {

    if (num % 2 == 0)
        even++;
    else
        odd++;
}

System.out.println("Even = " + even);
System.out.println("Odd = " + odd);
```

### Output

```
Even = 5
Odd = 0
```

---

# 9. Find Largest & Smallest Together ⭐⭐⭐⭐

### Example

```java
int min = arr[0];
int max = arr[0];

for (int num : arr) {

    if (num < min)
        min = num;

    if (num > max)
        max = num;
}

System.out.println("Min = " + min);
System.out.println("Max = " + max);
```

### Output

```
Min = 10
Max = 50
```

### Time Complexity

```
O(n)
```

---

# Summary

| Operation | Time | Space |
|-----------|------|-------|
| Forward Traversal | O(n) | O(1) |
| Reverse Traversal | O(n) | O(1) |
| Frequency Count | O(n) | O(n) |
| Find Minimum | O(n) | O(1) |
| Find Maximum | O(n) | O(1) |
| Find Sum | O(n) | O(1) |
| Find Average | O(n) | O(1) |
| Count Even/Odd | O(n) | O(1) |
| Find Min & Max | O(n) | O(1) |

---
# 📘 Searching Algorithms

Searching is the process of finding a target element in a collection of data.

---

# 1. Linear Search ⭐⭐⭐⭐⭐

## Definition

Linear Search checks each element one by one until the target is found.

### When to Use

- Unsorted Array
- Small Dataset
- Linked List

---

## Example

Array

```
[10, 20, 30, 40, 50]
```

Target

```
40
```

Search

```
10 ❌

20 ❌

30 ❌

40 ✅
```

---

## Algorithm

1. Start from index 0.
2. Compare current element with target.
3. If equal, return index.
4. Otherwise move to next element.
5. If end of array reached, return -1.

---

## Java Solution

```java
public static int linearSearch(int[] arr, int target) {

    for (int i = 0; i < arr.length; i++) {

        if (arr[i] == target) {
            return i;
        }

    }

    return -1;
}
```

---

## Dry Run

```
Array = [10,20,30,40,50]

Target = 40

i=0 → 10 ❌

i=1 → 20 ❌

i=2 → 30 ❌

i=3 → 40 ✅

Return 3
```

---

## Complexity

| Case | Complexity |
|------|------------|
| Best | O(1) |
| Average | O(n) |
| Worst | O(n) |
| Space | O(1) |

---

## Advantages

- Very simple
- Works on unsorted arrays
- No preprocessing

---

## Disadvantages

- Slow for large datasets

---

## Interview Questions

- Find an element
- Find first occurrence
- Find last occurrence
- Find maximum
- Find minimum

---

# 2. Binary Search ⭐⭐⭐⭐⭐

## Definition

Binary Search repeatedly divides the search space into half.

⚠️ Works **only on sorted arrays**.

---

## When to Use

- Sorted Array
- Large Dataset
- Fast Searching

---

## Example

Array

```
[10,20,30,40,50,60,70]
```

Target

```
50
```

Search

```
10 20 30 40 50 60 70

          ↑

Middle = 40

Target > 40

Search Right Half

50 60 70

    ↑

Middle = 60

Target < 60

Search Left Half

50

Found
```

---

## Algorithm

1. Find middle element.
2. Compare with target.
3. If equal → return.
4. If target is smaller → search left half.
5. If target is larger → search right half.
6. Repeat until found.

---
# Interview Tip ⭐⭐⭐⭐⭐

If the interviewer asks:

> **Why do we use `low + (high - low) / 2`?**

A strong answer is:

> "`(low + high) / 2` can overflow when `low` and `high` are large integers because their sum may exceed the maximum value of an `int`. `low + (high - low) / 2` computes the same midpoint without adding two potentially large numbers, making it overflow-safe."
## Java Solution (Iterative)

```java
public static int binarySearch(int[] arr, int target) {

    int low = 0;
    int high = arr.length - 1;

    while (low <= high) {

        int mid = low + (high - low) / 2;

        if (arr[mid] == target)
            return mid;

        if (arr[mid] < target)
            low = mid + 1;
        else
            high = mid - 1;
    }

    return -1;
}
```

---

## Java Solution (Recursive)

```java
public static int binarySearch(int[] arr, int low, int high, int target) {

    if (low > high)
        return -1;

    int mid = low + (high - low) / 2;

    if (arr[mid] == target)
        return mid;

    if (arr[mid] > target)
        return binarySearch(arr, low, mid - 1, target);

    return binarySearch(arr, mid + 1, high, target);
}
```

---

## Dry Run

```
Array

10 20 30 40 50 60 70

low = 0

high = 6

mid = 3

40

Target = 50

Move Right

-----------------

low = 4

high = 6

mid = 5

60

Move Left

-----------------

low = 4

high = 4

mid = 4

50

Found
```

---

## Complexity

| Case | Complexity |
|------|------------|
| Best | O(1) |
| Average | O(log n) |
| Worst | O(log n) |
| Space (Iterative) | O(1) |
| Space (Recursive) | O(log n) |

---

## Advantages

- Very fast
- Efficient for large datasets

---

## Disadvantages

- Requires sorted array

---

## Binary Search Variations ⭐⭐⭐⭐⭐

Learn these after basic Binary Search.

- Lower Bound
- Upper Bound
- First Occurrence
- Last Occurrence
- Search Insert Position
- Search in Rotated Sorted Array
- Find Peak Element
- Search in 2D Matrix
- Binary Search on Answer

---

# Comparison

| Feature | Linear Search | Binary Search |
|----------|---------------|---------------|
| Sorted Array Required | ❌ No | ✅ Yes |
| Best Case | O(1) | O(1) |
| Average Case | O(n) | O(log n) |
| Worst Case | O(n) | O(log n) |
| Space | O(1) | O(1) |
| Easy to Implement | ✅ | ✅ |
| Efficient for Large Data | ❌ | ✅ |

---

# Google Interview Strategy

### Step 1

Can I use Linear Search?

If **No**

↓

### Step 2

Is the array sorted?

If **Yes**

↓

Use Binary Search

↓

### Step 3

Can Binary Search solve the answer instead of searching the element?

↓

Binary Search on Answer

---

# Practice Problems

## Linear Search

- Find Element
- Find First Occurrence
- Find Last Occurrence
- Count Occurrences
- Search in Unsorted Array

---

## Binary Search

- Binary Search
- Search Insert Position
- First Bad Version
- Search in Rotated Array
- Find Peak Element
- Search a 2D Matrix
- Koko Eating Bananas (Binary Search on Answer)
- Capacity To Ship Packages
- Aggressive Cows
- Allocate Minimum Pages

---

# Revision Checklist

- [ ] Linear Search
- [ ] Binary Search
- [ ] Recursive Binary Search
- [ ] Lower Bound
- [ ] Upper Bound
- [ ] First Occurrence
- [ ] Last Occurrence
- [ ] Rotated Array
- [ ] Binary Search on Answer

