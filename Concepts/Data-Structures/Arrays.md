---
title: Arrays
category: Concepts/Data-Structures
tags:
  - data-structures
  - linear
  - fundamentals
difficulty: beginner
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html
  - https://docs.python.org/3/tutorial/datastructures.html
---

# Arrays

> [!summary]
> An array is a contiguous block of memory storing elements of the same type, accessible by index. It's the most fundamental data structure—fast for random access ($O(1)$) but inflexible for insertions and deletions ($O(n)$). Understanding arrays is essential because many other structures build upon them.

## Theory

### What Is an Array?

An array stores elements in **contiguous memory locations**, where each element can be accessed directly using its **index** (position number, typically starting at 0).

```
Index:    0     1     2     3     4
        ┌─────┬─────┬─────┬─────┬─────┐
Values: │  10 │  20 │  30 │  40 │  50 │
        └─────┴─────┴─────┴─────┴─────┘
Address: 1000  1004  1008  1012  1016  (4 bytes each for int)
```

**Key characteristics:**
- **Fixed size** — Size determined at creation (in most languages)
- **Homogeneous** — All elements same type (in typed languages)
- **Indexed** — Access any element by position in $O(1)$
- **Contiguous** — Elements stored next to each other in memory

### How It Works

The magic of $O(1)$ access comes from simple math:

```
element_address = base_address + (index × element_size)
```

To get `arr[3]` in an integer array starting at memory address 1000:
```
address = 1000 + (3 × 4) = 1012
```

One calculation, instant access—regardless of array size.

### Time Complexity

| Operation | Average | Worst | Notes |
|-----------|---------|-------|-------|
| Access by index | $O(1)$ | $O(1)$ | Direct memory calculation |
| Search (unsorted) | $O(n)$ | $O(n)$ | Must check each element |
| Search (sorted) | $O(\log n)$ | $O(\log n)$ | [[Binary-Search]] |
| Insert at end | $O(1)$* | $O(n)$ | *Amortized for dynamic arrays |
| Insert at position | $O(n)$ | $O(n)$ | Must shift elements |
| Delete | $O(n)$ | $O(n)$ | Must shift elements |

## Practical Examples

### Basic Array Operations (Java)

```java
public class ArrayBasics {
    public static void main(String[] args) {
        // Declaration and initialization
        int[] numbers = new int[5];           // Size 5, all zeros
        int[] primes = {2, 3, 5, 7, 11};      // Initialized with values
        
        // Access by index - O(1)
        int first = primes[0];    // 2
        int third = primes[2];    // 5
        
        // Modify element - O(1)
        primes[0] = 1;            // primes is now {1, 3, 5, 7, 11}
        
        // Get length
        int size = primes.length; // 5
        
        // Iterate - O(n)
        for (int i = 0; i < primes.length; i++) {
            System.out.println("Index " + i + ": " + primes[i]);
        }
        
        // Enhanced for loop
        for (int prime : primes) {
            System.out.println(prime);
        }
        
        // Common pitfall: ArrayIndexOutOfBoundsException
        // int error = primes[10];  // CRASH! Index 10 doesn't exist
    }
}
```

### Dynamic Arrays in Python

```python
# Python lists are dynamic arrays (not linked lists!)
# They automatically resize when needed

numbers = []  # Empty list

# Append - O(1) amortized
numbers.append(10)
numbers.append(20)
numbers.append(30)
print(numbers)  # [10, 20, 30]

# Access by index - O(1)
first = numbers[0]   # 10
last = numbers[-1]   # 30 (negative indexing!)

# Insert at position - O(n) due to shifting
numbers.insert(1, 15)  # [10, 15, 20, 30]

# Delete by index - O(n) due to shifting
del numbers[1]         # [10, 20, 30]

# Delete by value - O(n) search + O(n) shift
numbers.remove(20)     # [10, 30]

# Slicing - creates new array O(k) where k is slice size
first_two = numbers[0:2]
copy = numbers[:]       # Full copy
reversed_arr = numbers[::-1]  # Reverse

# List comprehension - Pythonic way to transform
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
evens = [x for x in range(20) if x % 2 == 0]  # [0, 2, 4, ..., 18]
```

### Common Array Algorithms (Java)

```java
public class ArrayAlgorithms {
    
    // Linear search - O(n)
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;  // Found at index i
            }
        }
        return -1;  // Not found
    }
    
    // Find maximum - O(n)
    public static int findMax(int[] arr) {
        if (arr.length == 0) {
            throw new IllegalArgumentException("Array is empty");
        }
        
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }
    
    // Reverse in-place - O(n) time, O(1) space
    public static void reverse(int[] arr) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            // Swap elements
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            
            left++;
            right--;
        }
    }
    
    // Two Sum - Find indices of two numbers that add to target
    // O(n) time using HashMap instead of O(n²) brute force
    public static int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> seen = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            
            if (seen.containsKey(complement)) {
                return new int[] { seen.get(complement), i };
            }
            
            seen.put(nums[i], i);
        }
        
        return new int[] {};  // No solution
    }
}
```

### 2D Arrays (Matrices)

```java
public class Matrix {
    public static void main(String[] args) {
        // 2D array declaration
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        
        // Access element at row 1, column 2
        int element = matrix[1][2];  // 6
        
        // Dimensions
        int rows = matrix.length;        // 3
        int cols = matrix[0].length;     // 3
        
        // Iterate through 2D array - O(rows × cols)
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
        
        // Transpose matrix - swap rows and columns
        int[][] transposed = new int[cols][rows];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                transposed[j][i] = matrix[i][j];
            }
        }
    }
}
```

```python
# Python 2D arrays with NumPy (preferred for numerical work)
import numpy as np

matrix = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

# Access element
element = matrix[1, 2]  # 6

# Shape
print(matrix.shape)  # (3, 3)

# Transpose
transposed = matrix.T

# Matrix operations
doubled = matrix * 2
squared = matrix ** 2
row_sums = matrix.sum(axis=1)  # Sum each row
col_sums = matrix.sum(axis=0)  # Sum each column
```

### Dynamic Array Implementation (Advanced)

```python
class DynamicArray:
    """
    Simplified implementation showing how dynamic arrays work.
    Python lists use similar logic internally.
    """
    
    def __init__(self, capacity: int = 8):
        self._capacity = capacity
        self._size = 0
        self._data = [None] * capacity  # Internal fixed-size array
    
    def __len__(self) -> int:
        return self._size
    
    def __getitem__(self, index: int):
        if index < 0 or index >= self._size:
            raise IndexError("Index out of bounds")
        return self._data[index]
    
    def append(self, value) -> None:
        """Add to end - O(1) amortized."""
        if self._size == self._capacity:
            self._resize(self._capacity * 2)  # Double capacity
        
        self._data[self._size] = value
        self._size += 1
    
    def _resize(self, new_capacity: int) -> None:
        """Create larger array and copy elements - O(n)."""
        new_data = [None] * new_capacity
        
        for i in range(self._size):
            new_data[i] = self._data[i]
        
        self._data = new_data
        self._capacity = new_capacity
    
    def insert(self, index: int, value) -> None:
        """Insert at position - O(n) due to shifting."""
        if self._size == self._capacity:
            self._resize(self._capacity * 2)
        
        # Shift elements right
        for i in range(self._size, index, -1):
            self._data[i] = self._data[i - 1]
        
        self._data[index] = value
        self._size += 1


# Usage
arr = DynamicArray()
for i in range(100):
    arr.append(i)  # Will resize multiple times internally
    
print(len(arr))    # 100
print(arr[50])     # 50
```

## Common Patterns

> [!tip] Two-Pointer Technique
> Many array problems can be solved efficiently with two pointers:
> - One pointer at start, one at end (reverse, two sum in sorted array)
> - Fast and slow pointers (cycle detection, finding middle)
> - Two arrays (merge sorted arrays)

> [!tip] Sliding Window
> For subarray problems, maintain a "window" that slides across:
> ```python
> # Maximum sum of k consecutive elements
> def max_sum_subarray(arr, k):
>     window_sum = sum(arr[:k])
>     max_sum = window_sum
>     
>     for i in range(k, len(arr)):
>         window_sum += arr[i] - arr[i - k]  # Slide window
>         max_sum = max(max_sum, window_sum)
>     
>     return max_sum
> ```

> [!warning] Off-By-One Errors
> The most common array bug! Remember:
> - Indices go from `0` to `length - 1`
> - `arr[arr.length]` is always out of bounds
> - When looping, use `< length` not `<= length`

## Edge Cases & Gotchas

- **Empty arrays** — Always check `length == 0` before accessing elements
- **Single element** — Some algorithms need special handling
- **All same values** — Test algorithms with duplicate values
- **Sorted vs unsorted** — Binary search only works on sorted arrays
- **Integer overflow** — Sum of large arrays can overflow; use `long`
- **Negative indices** — Python allows them; Java throws exception

## Related Topics

- [[Linked-Lists]] — Dynamic alternative when insertions are frequent
- [[Binary-Search]] — $O(\log n)$ search on sorted arrays
- [[Sorting-Algorithms]] — Organize array elements
- [[Dynamic-Programming]] — Many DP problems use arrays for memoization
- [[Strings]] — Often implemented as character arrays

## References

- [Oracle Java Arrays Tutorial](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)
- [Python Lists Documentation](https://docs.python.org/3/tutorial/datastructures.html)
- [NumPy Array Basics](https://numpy.org/doc/stable/user/basics.html)
