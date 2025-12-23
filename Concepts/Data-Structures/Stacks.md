---
title: Stacks
category: Concepts/Data-Structures
tags:
  - data-structures
  - linear
  - LIFO
  - fundamentals
difficulty: beginner
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://docs.oracle.com/javase/8/docs/api/java/util/Stack.html
  - https://www.geeksforgeeks.org/stack-data-structure/
---

# Stacks

> [!summary]
> A stack is a Last-In-First-Out (LIFO) data structure where elements are added and removed from the same end (the "top"). Think of a stack of plates—you can only add or remove from the top. Stacks are fundamental for function calls, undo operations, expression parsing, and backtracking algorithms.

## Theory

### What Is a Stack?

A stack restricts access to elements through two primary operations:
- **Push** — Add element to the top
- **Pop** — Remove and return the top element

```
Push 10, Push 20, Push 30:

    ┌────┐
    │ 30 │ ← top (last in)
    ├────┤
    │ 20 │
    ├────┤
    │ 10 │ ← bottom (first in)
    └────┘

Pop returns 30 (last in, first out)
```

**LIFO Principle:** The last element pushed is the first element popped.

### Core Operations

| Operation | Description | Time |
|-----------|-------------|------|
| `push(x)` | Add x to top | $O(1)$ |
| `pop()` | Remove and return top | $O(1)$ |
| `peek()` / `top()` | View top without removing | $O(1)$ |
| `isEmpty()` | Check if stack is empty | $O(1)$ |
| `size()` | Number of elements | $O(1)$ |

### Real-World Uses

- **Function call stack** — How programming languages track function calls
- **Undo/Redo** — Store previous states to reverse actions
- **Browser history** — Back button uses a stack
- **Expression parsing** — Matching parentheses, evaluating postfix
- **DFS traversal** — Depth-first search uses stack (explicit or call stack)
- **Backtracking** — Exploring paths, returning to previous states

## Practical Examples

### Stack Implementation (Java)

```java
import java.util.EmptyStackException;

public class Stack<T> {
    private static class Node<T> {
        T data;
        Node<T> next;
        
        Node(T data) {
            this.data = data;
        }
    }
    
    private Node<T> top;
    private int size;
    
    public void push(T data) {
        Node<T> newNode = new Node<>(data);
        newNode.next = top;
        top = newNode;
        size++;
    }
    
    public T pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        T data = top.data;
        top = top.next;
        size--;
        return data;
    }
    
    public T peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return top.data;
    }
    
    public boolean isEmpty() {
        return top == null;
    }
    
    public int size() {
        return size;
    }
}

// Usage
Stack<Integer> stack = new Stack<>();
stack.push(10);
stack.push(20);
stack.push(30);

System.out.println(stack.pop());   // 30
System.out.println(stack.peek());  // 20
System.out.println(stack.pop());   // 20
System.out.println(stack.pop());   // 10
```

### Using Built-in Stack (Java)

```java
import java.util.ArrayDeque;
import java.util.Deque;

// Prefer Deque over legacy Stack class
Deque<Integer> stack = new ArrayDeque<>();

stack.push(10);
stack.push(20);
stack.push(30);

while (!stack.isEmpty()) {
    System.out.println(stack.pop());  // 30, 20, 10
}
```

### Python Stack with List

```python
# Python lists work as stacks - append() and pop() are O(1)
stack = []

# Push
stack.append(10)
stack.append(20)
stack.append(30)

# Peek
top = stack[-1]  # 30

# Pop
print(stack.pop())  # 30
print(stack.pop())  # 20
print(stack.pop())  # 10

# Check empty
if not stack:
    print("Stack is empty")
```

### Classic Problem: Valid Parentheses

```java
// LeetCode #20: Valid Parentheses
// Given string with (){}[], determine if valid
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    
    Map<Character, Character> pairs = Map.of(
        ')', '(',
        '}', '{',
        ']', '['
    );
    
    for (char c : s.toCharArray()) {
        if (pairs.containsValue(c)) {
            // Opening bracket - push to stack
            stack.push(c);
        } else if (pairs.containsKey(c)) {
            // Closing bracket - must match top of stack
            if (stack.isEmpty() || stack.pop() != pairs.get(c)) {
                return false;
            }
        }
    }
    
    // Valid only if all brackets matched
    return stack.isEmpty();
}

// Examples:
// "()" → true
// "()[]{}" → true
// "(]" → false
// "([)]" → false
// "{[]}" → true
```

```python
def is_valid(s: str) -> bool:
    stack = []
    pairs = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in '({[':
            stack.append(char)
        elif char in ')}]':
            if not stack or stack.pop() != pairs[char]:
                return False
    
    return len(stack) == 0
```

### Evaluate Reverse Polish Notation (Postfix)

```python
def eval_rpn(tokens: list[str]) -> int:
    """
    Evaluate expression in Reverse Polish Notation.
    ["2", "1", "+", "3", "*"] = (2 + 1) * 3 = 9
    """
    stack = []
    operators = {
        '+': lambda a, b: a + b,
        '-': lambda a, b: a - b,
        '*': lambda a, b: a * b,
        '/': lambda a, b: int(a / b),  # Truncate toward zero
    }
    
    for token in tokens:
        if token in operators:
            b = stack.pop()  # Second operand
            a = stack.pop()  # First operand
            result = operators[token](a, b)
            stack.append(result)
        else:
            stack.append(int(token))
    
    return stack.pop()


# Examples:
print(eval_rpn(["2", "1", "+", "3", "*"]))  # 9
print(eval_rpn(["4", "13", "5", "/", "+"]))  # 6
```

### Min Stack (O(1) for all operations including getMin)

```java
// Design a stack that supports getMin() in O(1)
class MinStack {
    private Deque<Integer> stack;
    private Deque<Integer> minStack;  // Tracks minimums
    
    public MinStack() {
        stack = new ArrayDeque<>();
        minStack = new ArrayDeque<>();
    }
    
    public void push(int val) {
        stack.push(val);
        
        // Push to minStack if empty or val <= current min
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        }
    }
    
    public void pop() {
        int val = stack.pop();
        
        // If popped value was the min, pop from minStack too
        if (val == minStack.peek()) {
            minStack.pop();
        }
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```

### Daily Temperatures (Monotonic Stack)

```python
def daily_temperatures(temperatures: list[int]) -> list[int]:
    """
    For each day, find how many days until warmer temperature.
    Uses monotonic decreasing stack.
    
    Input: [73,74,75,71,69,72,76,73]
    Output: [1,1,4,2,1,1,0,0]
    """
    n = len(temperatures)
    result = [0] * n
    stack = []  # Store indices of unresolved temperatures
    
    for i, temp in enumerate(temperatures):
        # While current temp is warmer than stack top
        while stack and temp > temperatures[stack[-1]]:
            prev_idx = stack.pop()
            result[prev_idx] = i - prev_idx
        
        stack.append(i)
    
    return result
```

## Common Patterns

> [!tip] Monotonic Stack
> A stack that maintains elements in sorted order (increasing or decreasing). Useful for:
> - Next greater/smaller element problems
> - Daily temperatures
> - Histogram problems
> - Stock span problems

> [!tip] Two Stacks for Queue
> Implement a queue using two stacks:
> ```python
> class QueueWithStacks:
>     def __init__(self):
>         self.inbox = []   # For enqueue
>         self.outbox = []  # For dequeue
>     
>     def enqueue(self, x):
>         self.inbox.append(x)
>     
>     def dequeue(self):
>         if not self.outbox:
>             while self.inbox:
>                 self.outbox.append(self.inbox.pop())
>         return self.outbox.pop()
> ```

> [!warning] Stack Overflow
> Recursive functions use the call stack. Deep recursion can cause stack overflow. Use iterative solutions with explicit stacks for very deep recursions.

## Edge Cases & Gotchas

- **Empty stack** — Always check before `pop()` or `peek()`
- **Single element** — After pop, stack is empty
- **Stack overflow** — Limited memory for recursion
- **Order matters** — For expressions, operand order is significant

## Related Topics

- [[Queues]] — FIFO counterpart to LIFO stacks
- [[Recursion]] — Implicitly uses the call stack
- [[DFS]] — Depth-first search uses stack
- [[Linked-Lists]] — Common implementation for stacks
- [[Expression-Parsing]] — Infix to postfix conversion

## References

- [GeeksforGeeks - Stack](https://www.geeksforgeeks.org/stack-data-structure/)
- [Visualgo - Stack Visualization](https://visualgo.net/en/list)
- [LeetCode Stack Problems](https://leetcode.com/tag/stack/)
