---
title: Sorting Algorithms
category: Concepts/Algorithms
tags:
  - algorithms
  - sorting
  - interviews
  - fundamentals
difficulty: intermediate
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://www.toptal.com/developers/sorting-algorithms
  - https://visualgo.net/en/sorting
---

# Sorting Algorithms

> [!summary]
> Sorting algorithms arrange elements in a specific order (ascending/descending). They're fundamental to computer science—enabling binary search, simplifying problems, and appearing in countless applications. Understanding different sorting algorithms helps you choose the right tool: quick for general use, merge for stability guarantees, counting for integers in small ranges.

## Algorithm Comparison

| Algorithm      | Best           | Average        | Worst          | Space       | Stable | Notes                         |
| -------------- | -------------- | -------------- | -------------- | ----------- | ------ | ----------------------------- |
| Bubble Sort    | $O(n)$         | $O(n^2)$       | $O(n^2)$       | $O(1)$      | Yes    | Educational only              |
| Selection Sort | $O(n^2)$       | $O(n^2)$       | $O(n^2)$       | $O(1)$      | No     | Minimal swaps                 |
| Insertion Sort | $O(n)$         | $O(n^2)$       | $O(n^2)$       | $O(1)$      | Yes    | Great for small/nearly sorted |
| Merge Sort     | $O(n \log n)$  | $O(n \log n)$  | $O(n \log n)$  | $O(n)$      | Yes    | Guaranteed performance        |
| Quick Sort     | $O(n \log n)$  | $O(n \log n)$  | $O(n^2)$       | $O(\log n)$ | No     | Fastest in practice           |
| Heap Sort      | $O(n \log n)$  | $O(n \log n)$  | $O(n \log n)$  | $O(1)$      | No     | In-place, no extra memory     |
| Counting Sort  | $O(n + k)$     | $O(n + k)$     | $O(n + k)$     | $O(k)$      | Yes    | Integers in range [0, k]      |
| Radix Sort     | $O(d \cdot n)$ | $O(d \cdot n)$ | $O(d \cdot n)$ | $O(n + k)$  | Yes    | d = digits, k = base          |

> [!info] Stability
> A stable sort maintains the relative order of equal elements. Important when sorting by multiple keys (e.g., sort by name, then by age—stable sort preserves name order for same ages).

## Simple Sorts ($O(n^2)$)

### Bubble Sort

Repeatedly swap adjacent elements if out of order. Largest "bubbles up" to the end.

```python
def bubble_sort(arr: list[int]) -> None:
    n = len(arr)
    
    for i in range(n):
        swapped = False
        
        for j in range(n - 1 - i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        
        if not swapped:  # Optimization: already sorted
            break
```

```
[5, 3, 8, 4, 2] → compare 5,3, swap
[3, 5, 8, 4, 2] → compare 5,8, no swap
[3, 5, 8, 4, 2] → compare 8,4, swap
[3, 5, 4, 8, 2] → compare 8,2, swap
[3, 5, 4, 2, 8] → 8 is now in place, repeat...
```

> [!warning] Never Use Bubble Sort in Production
> It's the slowest sorting algorithm. Only useful for educational purposes.

### Selection Sort

Find minimum element, swap it to the front. Repeat for remaining elements.

```python
def selection_sort(arr: list[int]) -> None:
    n = len(arr)
    
    for i in range(n):
        min_idx = i
        
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
```

```
[5, 3, 8, 4, 2] → min is 2 at index 4, swap with index 0
[2, 3, 8, 4, 5] → min is 3 at index 1 (already in place)
[2, 3, 8, 4, 5] → min is 4 at index 3, swap with index 2
[2, 3, 4, 8, 5] → min is 5 at index 4, swap with index 3
[2, 3, 4, 5, 8] → sorted!
```

### Insertion Sort

Build sorted array one element at a time by inserting each element into its correct position.

```python
def insertion_sort(arr: list[int]) -> None:
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        
        # Shift elements greater than key
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        
        arr[j + 1] = key
```

```java
public void insertionSort(int[] arr) {
    for (int i = 1; i < arr.length; i++) {
        int key = arr[i];
        int j = i - 1;
        
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        
        arr[j + 1] = key;
    }
}
```

> [!tip] When to Use Insertion Sort
> - Small arrays (< 50 elements)
> - Nearly sorted data ($O(n)$ best case!)
> - Online sorting (sorting as data arrives)
> - As the base case for hybrid sorts (Timsort, Introsort)

## Efficient Sorts ($O(n \log n)$)

### Merge Sort

Divide array in half, recursively sort each half, merge sorted halves.

```python
def merge_sort(arr: list[int]) -> list[int]:
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)


def merge(left: list[int], right: list[int]) -> list[int]:
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    result.extend(left[i:])
    result.extend(right[j:])
    
    return result
```

```java
public void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        
        merge(arr, left, mid, right);
    }
}

private void merge(int[] arr, int left, int mid, int right) {
    int[] leftArr = Arrays.copyOfRange(arr, left, mid + 1);
    int[] rightArr = Arrays.copyOfRange(arr, mid + 1, right + 1);
    
    int i = 0, j = 0, k = left;
    
    while (i < leftArr.length && j < rightArr.length) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k++] = leftArr[i++];
        } else {
            arr[k++] = rightArr[j++];
        }
    }
    
    while (i < leftArr.length) arr[k++] = leftArr[i++];
    while (j < rightArr.length) arr[k++] = rightArr[j++];
}
```

```
[38, 27, 43, 3, 9, 82, 10]
        ↓ divide
[38, 27, 43, 3] | [9, 82, 10]
        ↓ divide
[38, 27] | [43, 3] | [9, 82] | [10]
        ↓ divide
[38] [27] [43] [3] [9] [82] [10]
        ↓ merge
[27, 38] | [3, 43] | [9, 82] | [10]
        ↓ merge
[3, 27, 38, 43] | [9, 10, 82]
        ↓ merge
[3, 9, 10, 27, 38, 43, 82]
```

> [!tip] Merge Sort Advantages
> - Guaranteed $O(n \log n)$ (no worst case degradation)
> - Stable (preserves order of equal elements)
> - Great for linked lists (no random access needed)
> - Easily parallelizable

### Quick Sort

Choose a pivot, partition array so elements < pivot are left, > pivot are right. Recursively sort partitions.

```python
def quick_sort(arr: list[int], low: int = 0, high: int = None) -> None:
    if high is None:
        high = len(arr) - 1
    
    if low < high:
        pivot_idx = partition(arr, low, high)
        quick_sort(arr, low, pivot_idx - 1)
        quick_sort(arr, pivot_idx + 1, high)


def partition(arr: list[int], low: int, high: int) -> int:
    pivot = arr[high]  # Choose last element as pivot
    i = low - 1
    
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```

```java
public void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pivotIdx = partition(arr, low, high);
        quickSort(arr, low, pivotIdx - 1);
        quickSort(arr, pivotIdx + 1, high);
    }
}

private int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr, i, j);
        }
    }
    
    swap(arr, i + 1, high);
    return i + 1;
}
```

```
[3, 7, 8, 5, 2, 1, 9, 5, 4], pivot = 4
          ↓ partition
[3, 2, 1, 4, 7, 8, 9, 5, 5]
       ↑     ↑
    < pivot  > pivot
          ↓ recursively sort both sides
[1, 2, 3] [4] [5, 5, 7, 8, 9]
          ↓
[1, 2, 3, 4, 5, 5, 7, 8, 9]
```

> [!warning] Quick Sort Worst Case
> If pivot is always min or max (sorted array + first/last pivot), degrades to $O(n^2)$. Use randomized pivot or median-of-three to avoid.

### Heap Sort

Build max-heap, repeatedly extract maximum to end of array.

```python
def heap_sort(arr: list[int]) -> None:
    n = len(arr)
    
    # Build max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)
    
    # Extract elements one by one
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]  # Move max to end
        heapify(arr, i, 0)


def heapify(arr: list[int], n: int, i: int) -> None:
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    
    if left < n and arr[left] > arr[largest]:
        largest = left
    
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)
```

## Non-Comparison Sorts

### Counting Sort

Count occurrences of each value, reconstruct sorted array from counts.

```python
def counting_sort(arr: list[int]) -> list[int]:
    if not arr:
        return arr
    
    min_val, max_val = min(arr), max(arr)
    range_size = max_val - min_val + 1
    
    # Count occurrences
    count = [0] * range_size
    for num in arr:
        count[num - min_val] += 1
    
    # Cumulative count (for stability)
    for i in range(1, range_size):
        count[i] += count[i - 1]
    
    # Build output array (stable - go backwards)
    output = [0] * len(arr)
    for num in reversed(arr):
        idx = count[num - min_val] - 1
        output[idx] = num
        count[num - min_val] -= 1
    
    return output
```

> [!tip] When to Use Counting Sort
> - Integer keys in known, small range
> - $O(n + k)$ where k is the range
> - Great when k ≈ n (range similar to count)

## Using Built-in Sorts

```java
// Java - Arrays.sort uses dual-pivot quicksort for primitives
int[] arr = {3, 1, 4, 1, 5, 9};
Arrays.sort(arr);

// For objects, uses Timsort (stable)
Integer[] objArr = {3, 1, 4, 1, 5, 9};
Arrays.sort(objArr);

// Custom comparator
Arrays.sort(objArr, Collections.reverseOrder());
Arrays.sort(objArr, (a, b) -> b - a);

// List sorting
List<Integer> list = Arrays.asList(3, 1, 4, 1, 5, 9);
Collections.sort(list);
list.sort(Comparator.reverseOrder());
```

```python
# Python uses Timsort (hybrid of merge sort + insertion sort)
arr = [3, 1, 4, 1, 5, 9]

# In-place sort
arr.sort()

# Return new sorted list
sorted_arr = sorted(arr)

# Reverse order
arr.sort(reverse=True)
sorted(arr, reverse=True)

# Custom key function
words = ["banana", "apple", "cherry"]
words.sort(key=len)  # Sort by length
sorted(words, key=str.lower)  # Case-insensitive

# Sort objects by attribute
people.sort(key=lambda p: p.age)
from operator import attrgetter
people.sort(key=attrgetter('age'))

# Multiple keys
people.sort(key=lambda p: (p.age, p.name))
```

## Common Patterns

> [!tip] Choosing a Sorting Algorithm
> - **Default**: Use built-in sort (Timsort/Introsort)
> - **Need stability**: Merge sort
> - **Memory constrained**: Heap sort or quick sort
> - **Nearly sorted data**: Insertion sort
> - **Small range integers**: Counting sort
> - **Strings/large keys**: Radix sort

> [!tip] Sorting Interview Questions
> Most interview "sorting" questions don't ask you to implement a sort—they ask you to USE sorting as part of the solution:
> - Sort first, then binary search
> - Sort first, then two pointers
> - Custom comparator for special ordering

## Edge Cases & Gotchas

- **Empty array** — Handle gracefully
- **Single element** — Already sorted
- **All same elements** — Should work correctly
- **Already sorted** — Quick sort degrades without good pivot
- **Reverse sorted** — Same issue for quick sort
- **Integer overflow** — Be careful with custom comparators: `(a, b) -> a - b` can overflow

## Related Topics

- [[Binary-Search]] — Requires sorted input
- [[Heaps]] — Used in heap sort
- [[Divide-and-Conquer]] — Merge sort and quick sort paradigm
- [[Big-O-Notation]] — Analyzing sort complexity

## References

- [Sorting Algorithm Animations](https://www.toptal.com/developers/sorting-algorithms)
- [Visualgo Sorting](https://visualgo.net/en/sorting)
- [Python Timsort](https://en.wikipedia.org/wiki/Timsort)
