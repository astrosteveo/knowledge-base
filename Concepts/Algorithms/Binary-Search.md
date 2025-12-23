---
title: Binary Search
category: Concepts/Algorithms
tags:
  - algorithms
  - searching
  - divide-and-conquer
  - interviews
difficulty: beginner
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://www.topcoder.com/thrive/articles/Binary%20Search
  - https://leetcode.com/explore/learn/card/binary-search/
---

# Binary Search

> [!summary]
> Binary search finds a target value in a **sorted** array by repeatedly dividing the search space in half. Each comparison eliminates half the remaining elements, achieving $O(\log n)$ time—dramatically faster than linear search for large datasets. If your array is sorted and you're doing linear search, you're likely missing an optimization.

## Theory

### How It Works

1. Start with the entire sorted array
2. Compare target to middle element
3. If equal, found it!
4. If target < middle, search left half
5. If target > middle, search right half
6. Repeat until found or search space exhausted

```
Find 23 in [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]

Step 1: mid = 16, target > 16, search right
        [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
                       ↑
                      mid

Step 2: mid = 56, target < 56, search left
        [23, 38, 56, 72, 91]
              ↑
             mid

Step 3: mid = 23, target == 23, FOUND!
        [23, 38]
         ↑
        mid
```

### Time Complexity

| Operation | Time |
|-----------|------|
| Search | $O(\log n)$ |
| Space | $O(1)$ iterative, $O(\log n)$ recursive |

Why $\log n$? Each step halves the search space:
- n → n/2 → n/4 → n/8 → ... → 1
- Number of steps = $\log_2 n$

For 1 billion elements: only ~30 comparisons!

## Practical Examples

### Basic Binary Search (Iterative)

```java
public int binarySearch(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Prevents overflow
        
        if (arr[mid] == target) {
            return mid;  // Found!
        } else if (arr[mid] < target) {
            left = mid + 1;  // Search right half
        } else {
            right = mid - 1;  // Search left half
        }
    }
    
    return -1;  // Not found
}
```

```python
def binary_search(arr: list[int], target: int) -> int:
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1
```

### Binary Search (Recursive)

```java
public int binarySearchRecursive(int[] arr, int target, int left, int right) {
    if (left > right) {
        return -1;  // Base case: not found
    }
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        return binarySearchRecursive(arr, target, mid + 1, right);
    } else {
        return binarySearchRecursive(arr, target, left, mid - 1);
    }
}
```

### Find First/Last Occurrence

```python
def find_first(arr: list[int], target: int) -> int:
    """Find leftmost (first) occurrence of target."""
    left, right = 0, len(arr) - 1
    result = -1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            result = mid       # Found, but keep searching left
            right = mid - 1
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return result


def find_last(arr: list[int], target: int) -> int:
    """Find rightmost (last) occurrence of target."""
    left, right = 0, len(arr) - 1
    result = -1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            result = mid       # Found, but keep searching right
            left = mid + 1
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return result


# Count occurrences
def count_occurrences(arr: list[int], target: int) -> int:
    first = find_first(arr, target)
    if first == -1:
        return 0
    last = find_last(arr, target)
    return last - first + 1
```

### Search Insert Position

```python
def search_insert(nums: list[int], target: int) -> int:
    """
    Find index where target is or would be inserted.
    LeetCode #35
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return left  # Insert position


# Examples:
# [1,3,5,6], target=5 → 2 (found at index 2)
# [1,3,5,6], target=2 → 1 (insert at index 1)
# [1,3,5,6], target=7 → 4 (insert at end)
```

### Binary Search on Answer (Advanced)

```python
def sqrt_int(x: int) -> int:
    """
    Find floor(sqrt(x)) using binary search.
    Search space: possible answers [0, x]
    """
    if x < 2:
        return x
    
    left, right = 1, x // 2
    
    while left <= right:
        mid = (left + right) // 2
        
        if mid * mid == x:
            return mid
        elif mid * mid < x:
            left = mid + 1
        else:
            right = mid - 1
    
    return right  # Floor of sqrt


def min_eating_speed(piles: list[int], h: int) -> int:
    """
    Koko eating bananas - LeetCode #875
    Find minimum speed k to eat all piles in h hours.
    Binary search on the answer (speed).
    """
    def can_finish(k: int) -> bool:
        hours = sum((pile + k - 1) // k for pile in piles)  # Ceiling division
        return hours <= h
    
    left, right = 1, max(piles)
    
    while left < right:
        mid = (left + right) // 2
        
        if can_finish(mid):
            right = mid      # Try smaller speed
        else:
            left = mid + 1   # Need faster speed
    
    return left
```

### Rotated Sorted Array

```python
def search_rotated(nums: list[int], target: int) -> int:
    """
    Search in rotated sorted array - LeetCode #33
    [4,5,6,7,0,1,2] was [0,1,2,4,5,6,7] rotated
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid
        
        # Left half is sorted
        if nums[left] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half is sorted
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    
    return -1
```

### Using Built-in Functions

```java
import java.util.Arrays;

int[] arr = {1, 3, 5, 7, 9};
int index = Arrays.binarySearch(arr, 5);  // Returns 2

// If not found, returns (-(insertion point) - 1)
int notFound = Arrays.binarySearch(arr, 6);  // Returns -4 (would insert at index 3)
```

```python
import bisect

arr = [1, 3, 5, 7, 9]

# Find insertion point (leftmost)
bisect.bisect_left(arr, 5)   # 2 (index where 5 is)
bisect.bisect_left(arr, 6)   # 3 (index where 6 would be inserted)

# Find insertion point (rightmost)
bisect.bisect_right(arr, 5)  # 3 (after existing 5s)

# Insert while maintaining sorted order
bisect.insort(arr, 6)  # arr is now [1, 3, 5, 6, 7, 9]
```

## Common Patterns

> [!tip] Template for Most Binary Search Problems
> ```python
> def binary_search_template(arr, condition):
>     left, right = 0, len(arr) - 1  # or appropriate bounds
>     
>     while left <= right:  # or left < right for some variants
>         mid = (left + right) // 2
>         
>         if condition(mid):
>             # Found or adjust based on problem
>             right = mid - 1  # or left = mid + 1
>         else:
>             left = mid + 1   # or right = mid - 1
>     
>     return left  # or right, or -1, depending on problem
> ```

> [!tip] Binary Search on Answer
> When the problem asks for "minimum/maximum X such that condition is satisfied":
> 1. Define the search space (possible answers)
> 2. Write a function to check if an answer works
> 3. Binary search to find the boundary

> [!warning] Common Bugs
> - **Integer overflow**: Use `mid = left + (right - left) / 2` instead of `(left + right) / 2`
> - **Infinite loops**: Ensure left and right converge (check boundary updates)
> - **Off-by-one**: Carefully decide `left <= right` vs `left < right`
> - **Forgetting sorted requirement**: Binary search ONLY works on sorted/monotonic data!

## Edge Cases & Gotchas

- **Empty array** — Check `arr.length == 0`
- **Single element** — Works correctly
- **All same elements** — First/last variants handle this
- **Target not in array** — Return -1 or insertion point
- **Duplicates** — Use first/last occurrence variants
- **Array not sorted** — Binary search will give wrong results!

## Related Topics

- [[Linear-Search]] — $O(n)$ alternative for unsorted data
- [[Sorting-Algorithms]] — Must sort first if data is unsorted
- [[Divide-and-Conquer]] — Binary search is a D&C algorithm
- [[Trees]] — BST uses binary search principle

## References

- [LeetCode Binary Search Study Plan](https://leetcode.com/explore/learn/card/binary-search/)
- [TopCoder Binary Search Tutorial](https://www.topcoder.com/thrive/articles/Binary%20Search)
- [Visualgo - Binary Search](https://visualgo.net/en/sorting)
