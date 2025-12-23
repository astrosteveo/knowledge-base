---
title: Recursion
category: Concepts/Algorithms
tags:
  - algorithms
  - fundamentals
  - interviews
  - problem-solving
difficulty: intermediate
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://www.geeksforgeeks.org/recursion/
  - https://www.khanacademy.org/computing/computer-science/algorithms/recursive-algorithms/a/recursion
---

# Recursion

> [!summary]
> Recursion is when a function calls itself to solve a problem by breaking it into smaller, similar subproblems. It's a powerful technique that provides elegant solutions to problems like tree traversal, divide-and-conquer algorithms, and backtracking. Mastering recursion is essential—it's the foundation for understanding trees, graphs, dynamic programming, and many classic algorithms.

## Theory

### What Is Recursion?

A recursive function has two essential parts:

1. **Base case** — The condition that stops recursion (prevents infinite loops)
2. **Recursive case** — The function calls itself with a smaller/simpler input

```
factorial(5)
= 5 × factorial(4)
= 5 × 4 × factorial(3)
= 5 × 4 × 3 × factorial(2)
= 5 × 4 × 3 × 2 × factorial(1)
= 5 × 4 × 3 × 2 × 1  ← Base case!
= 120
```

### How the Call Stack Works

Each recursive call adds a frame to the call stack:

```
factorial(4) called
├── factorial(3) called
│   ├── factorial(2) called
│   │   ├── factorial(1) called
│   │   │   └── returns 1 (base case)
│   │   └── returns 2 × 1 = 2
│   └── returns 3 × 2 = 6
└── returns 4 × 6 = 24
```

> [!warning] Stack Overflow
> Each call uses memory. Too many recursive calls cause stack overflow. Python default limit is ~1000 calls; Java varies by JVM settings.

### Recursion vs Iteration

| Aspect | Recursion | Iteration |
|--------|-----------|-----------|
| Code | Often cleaner, more intuitive | Can be more verbose |
| Memory | Uses call stack ($O(n)$ space) | Usually $O(1)$ space |
| Speed | Function call overhead | Generally faster |
| Best for | Trees, divide-and-conquer | Simple loops, performance-critical |

## Practical Examples

### Factorial

```python
# Recursive
def factorial(n: int) -> int:
    # Base case
    if n <= 1:
        return 1
    
    # Recursive case
    return n * factorial(n - 1)


# Iterative equivalent
def factorial_iterative(n: int) -> int:
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result
```

```java
public int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

### Fibonacci

```python
# Naive recursive - O(2^n) time! Don't use this!
def fib_naive(n: int) -> int:
    if n <= 1:
        return n
    return fib_naive(n - 1) + fib_naive(n - 2)


# Memoized recursive - O(n) time
def fib_memo(n: int, memo: dict = None) -> int:
    if memo is None:
        memo = {}
    
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo)
    return memo[n]


# Iterative - O(n) time, O(1) space
def fib_iterative(n: int) -> int:
    if n <= 1:
        return n
    
    prev, curr = 0, 1
    for _ in range(2, n + 1):
        prev, curr = curr, prev + curr
    
    return curr
```

> [!warning] Exponential Recursion
> Naive Fibonacci has $O(2^n)$ time because it recomputes the same values repeatedly. Always consider memoization for overlapping subproblems!

### Sum of Array

```python
def array_sum(arr: list[int]) -> int:
    # Base case: empty array
    if not arr:
        return 0
    
    # Recursive: first element + sum of rest
    return arr[0] + array_sum(arr[1:])


# With index (more efficient - no slicing)
def array_sum_idx(arr: list[int], idx: int = 0) -> int:
    if idx >= len(arr):
        return 0
    return arr[idx] + array_sum_idx(arr, idx + 1)
```

### Reverse String

```python
def reverse_string(s: str) -> str:
    # Base case
    if len(s) <= 1:
        return s
    
    # Recursive: last char + reverse of rest
    return s[-1] + reverse_string(s[:-1])


# More efficient with indices
def reverse_helper(s: list[str], left: int, right: int) -> None:
    if left >= right:
        return
    
    s[left], s[right] = s[right], s[left]
    reverse_helper(s, left + 1, right - 1)
```

### Binary Search (Recursive)

```python
def binary_search(arr: list[int], target: int, left: int, right: int) -> int:
    # Base case: not found
    if left > right:
        return -1
    
    mid = (left + right) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search(arr, target, mid + 1, right)
    else:
        return binary_search(arr, target, left, mid - 1)
```

### Tree Traversal

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None


def inorder(root: TreeNode) -> list[int]:
    if not root:
        return []
    
    return inorder(root.left) + [root.val] + inorder(root.right)


def tree_height(root: TreeNode) -> int:
    if not root:
        return 0
    
    return 1 + max(tree_height(root.left), tree_height(root.right))


def count_nodes(root: TreeNode) -> int:
    if not root:
        return 0
    
    return 1 + count_nodes(root.left) + count_nodes(root.right)
```

### Generate All Subsets (Backtracking)

```python
def subsets(nums: list[int]) -> list[list[int]]:
    result = []
    
    def backtrack(start: int, current: list[int]):
        result.append(current[:])  # Add copy of current subset
        
        for i in range(start, len(nums)):
            current.append(nums[i])      # Choose
            backtrack(i + 1, current)    # Explore
            current.pop()                 # Un-choose (backtrack)
    
    backtrack(0, [])
    return result


# subsets([1, 2, 3]) returns:
# [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

### Permutations

```python
def permutations(nums: list[int]) -> list[list[int]]:
    result = []
    
    def backtrack(current: list[int], remaining: set):
        if not remaining:
            result.append(current[:])
            return
        
        for num in list(remaining):
            current.append(num)
            remaining.remove(num)
            backtrack(current, remaining)
            current.pop()
            remaining.add(num)
    
    backtrack([], set(nums))
    return result


# permutations([1, 2, 3]) returns:
# [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]
```

### Merge Sort (Divide and Conquer)

```python
def merge_sort(arr: list[int]) -> list[int]:
    # Base case
    if len(arr) <= 1:
        return arr
    
    # Divide
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    # Conquer (merge)
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

### Tail Recursion

```python
# Regular recursion - builds up stack
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)  # Must wait for recursive call


# Tail recursion - last operation is the recursive call
def factorial_tail(n, accumulator=1):
    if n <= 1:
        return accumulator
    return factorial_tail(n - 1, n * accumulator)  # Nothing to do after


# Note: Python doesn't optimize tail recursion, but some languages do (Scheme, Erlang)
```

## Common Patterns

> [!tip] The Recursion Template
> ```python
> def solve(problem):
>     # 1. Base case(s)
>     if is_base_case(problem):
>         return base_solution
>     
>     # 2. Recursive case
>     smaller_problem = reduce(problem)
>     result = solve(smaller_problem)
>     
>     # 3. Combine (if needed)
>     return combine(result, problem)
> ```

> [!tip] Converting to Iteration
> Most recursion can be converted to iteration using an explicit stack:
> ```python
> # Recursive
> def traverse(node):
>     if not node:
>         return
>     process(node)
>     traverse(node.left)
>     traverse(node.right)
> 
> # Iterative with stack
> def traverse_iterative(root):
>     stack = [root] if root else []
>     while stack:
>         node = stack.pop()
>         process(node)
>         if node.right:
>             stack.append(node.right)
>         if node.left:
>             stack.append(node.left)
> ```

> [!tip] Debugging Recursion
> 1. Add print statements showing input at each call
> 2. Draw the recursion tree on paper
> 3. Check base case first—most bugs are there
> 4. Verify each call gets closer to base case

## Time Complexity Analysis

| Pattern | Recurrence | Complexity |
|---------|------------|------------|
| Linear (factorial) | $T(n) = T(n-1) + O(1)$ | $O(n)$ |
| Binary (binary search) | $T(n) = T(n/2) + O(1)$ | $O(\log n)$ |
| Tree traversal | $T(n) = 2T(n/2) + O(1)$ | $O(n)$ |
| Merge sort | $T(n) = 2T(n/2) + O(n)$ | $O(n \log n)$ |
| Naive Fibonacci | $T(n) = T(n-1) + T(n-2)$ | $O(2^n)$ |
| Subsets/Permutations | — | $O(2^n)$ or $O(n!)$ |

## Edge Cases & Gotchas

- **Missing base case** — Infinite recursion, stack overflow
- **Base case not reachable** — Check that recursive calls move toward base case
- **Off-by-one** — Common in index-based recursion
- **Modifying shared state** — Be careful with mutable arguments
- **Excessive memory** — Deep recursion uses lots of stack space

> [!warning] When NOT to Use Recursion
> - Simple iteration would suffice (summing array)
> - Very deep recursion expected (use iteration + explicit stack)
> - Performance-critical code (function call overhead)
> - Naive implementation has exponential complexity (use memoization or DP)

## Related Topics

- [[Stacks]] — Call stack is literally a stack
- [[Trees]] — Most tree problems use recursion
- [[Dynamic-Programming]] — Recursion + memoization
- [[Backtracking]] — Recursion for exploring possibilities
- [[Divide-and-Conquer]] — Recursive problem decomposition

## References

- [Khan Academy - Recursion](https://www.khanacademy.org/computing/computer-science/algorithms/recursive-algorithms)
- [GeeksforGeeks - Recursion](https://www.geeksforgeeks.org/recursion/)
- [Visualgo - Recursion Tree](https://visualgo.net/en/recursion)
