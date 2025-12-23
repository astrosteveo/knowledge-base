---
title: Linked Lists
category: Concepts/Data-Structures
tags:
  - data-structures
  - linear
  - fundamentals
  - interviews
difficulty: beginner
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://www.geeksforgeeks.org/data-structures/linked-list/
  - https://visualgo.net/en/list
---

# Linked Lists

> [!summary]
> A linked list is a linear data structure where elements (nodes) are connected via pointers rather than stored contiguously. Unlike [[Arrays]], linked lists allow efficient insertions and deletions ($O(1)$ at known positions) but sacrifice random access ($O(n)$ to reach any element). They're fundamental for understanding pointers and are heavily tested in interviews.

## Theory

### What Is a Linked List?

A linked list consists of **nodes**, where each node contains:
1. **Data** — The value stored
2. **Pointer(s)** — Reference to the next (and sometimes previous) node

```
Singly Linked List:
┌───────────┐    ┌───────────┐    ┌───────────┐    ┌───────────┐
│ 10 │ next─┼───►│ 20 │ next─┼───►│ 30 │ next─┼───►│ 40 │ null │
└───────────┘    └───────────┘    └───────────┘    └───────────┘
     head                                               tail

Doubly Linked List:
         ┌───────────────┐    ┌───────────────┐    ┌───────────────┐
null ◄───┼─prev │ 10 │ next─┼───►│ 20 │ next─┼───►│ 30 │ null │
         └───────────────┘◄───┼─prev         └───────────────┘◄───┼─prev
              head                                     tail
```

### Types of Linked Lists

| Type | Description | Use Case |
|------|-------------|----------|
| **Singly Linked** | Each node points to next only | Simple, forward traversal |
| **Doubly Linked** | Nodes point to both next and previous | Bidirectional traversal, LRU cache |
| **Circular** | Last node points back to first | Round-robin scheduling, playlists |

### Time Complexity

| Operation | Singly | Doubly | Notes |
|-----------|--------|--------|-------|
| Access by index | $O(n)$ | $O(n)$ | Must traverse from head |
| Search | $O(n)$ | $O(n)$ | Must traverse list |
| Insert at head | $O(1)$ | $O(1)$ | Just update pointers |
| Insert at tail | $O(n)$* | $O(1)$ | *$O(1)$ if tail pointer maintained |
| Insert at position | $O(n)$ | $O(n)$ | Must traverse to position |
| Delete at head | $O(1)$ | $O(1)$ | Just update head |
| Delete at tail | $O(n)$ | $O(1)$ | Singly needs to find second-to-last |
| Delete by value | $O(n)$ | $O(n)$ | Must search first |

### Arrays vs Linked Lists

| Aspect | Array | Linked List |
|--------|-------|-------------|
| Memory layout | Contiguous | Scattered |
| Random access | $O(1)$ | $O(n)$ |
| Insert/delete | $O(n)$ | $O(1)$* |
| Memory overhead | Low | Higher (pointers) |
| Cache performance | Excellent | Poor |
| Size | Fixed or resize needed | Dynamic |

*At known position

## Practical Examples

### Singly Linked List (Java)

```java
public class SinglyLinkedList<T> {
    
    private static class Node<T> {
        T data;
        Node<T> next;
        
        Node(T data) {
            this.data = data;
            this.next = null;
        }
    }
    
    private Node<T> head;
    private Node<T> tail;
    private int size;
    
    public SinglyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
    
    // Add to end - O(1) with tail pointer
    public void append(T data) {
        Node<T> newNode = new Node<>(data);
        
        if (head == null) {
            head = newNode;
            tail = newNode;
        } else {
            tail.next = newNode;
            tail = newNode;
        }
        size++;
    }
    
    // Add to beginning - O(1)
    public void prepend(T data) {
        Node<T> newNode = new Node<>(data);
        newNode.next = head;
        head = newNode;
        
        if (tail == null) {
            tail = newNode;
        }
        size++;
    }
    
    // Insert at index - O(n)
    public void insertAt(int index, T data) {
        if (index < 0 || index > size) {
            throw new IndexOutOfBoundsException();
        }
        
        if (index == 0) {
            prepend(data);
            return;
        }
        
        if (index == size) {
            append(data);
            return;
        }
        
        // Traverse to position before insertion point
        Node<T> current = head;
        for (int i = 0; i < index - 1; i++) {
            current = current.next;
        }
        
        Node<T> newNode = new Node<>(data);
        newNode.next = current.next;
        current.next = newNode;
        size++;
    }
    
    // Delete first occurrence - O(n)
    public boolean delete(T data) {
        if (head == null) return false;
        
        // Special case: delete head
        if (head.data.equals(data)) {
            head = head.next;
            if (head == null) tail = null;
            size--;
            return true;
        }
        
        // Find node before the one to delete
        Node<T> current = head;
        while (current.next != null && !current.next.data.equals(data)) {
            current = current.next;
        }
        
        if (current.next == null) return false;  // Not found
        
        // Remove the node
        current.next = current.next.next;
        if (current.next == null) tail = current;
        size--;
        return true;
    }
    
    // Search - O(n)
    public boolean contains(T data) {
        Node<T> current = head;
        while (current != null) {
            if (current.data.equals(data)) return true;
            current = current.next;
        }
        return false;
    }
    
    // Get by index - O(n)
    public T get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException();
        }
        
        Node<T> current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.data;
    }
    
    public int size() { return size; }
    public boolean isEmpty() { return size == 0; }
}
```

### Python Implementation with Iteration Support

```python
from typing import TypeVar, Generic, Optional, Iterator

T = TypeVar('T')


class Node(Generic[T]):
    def __init__(self, data: T):
        self.data = data
        self.next: Optional[Node[T]] = None


class LinkedList(Generic[T]):
    def __init__(self):
        self.head: Optional[Node[T]] = None
        self.tail: Optional[Node[T]] = None
        self._size = 0
    
    def __len__(self) -> int:
        return self._size
    
    def __iter__(self) -> Iterator[T]:
        """Makes the list iterable: for item in my_list"""
        current = self.head
        while current:
            yield current.data
            current = current.next
    
    def __str__(self) -> str:
        items = [str(item) for item in self]
        return " -> ".join(items) + " -> None"
    
    def append(self, data: T) -> None:
        """Add to end - O(1)"""
        new_node = Node(data)
        
        if not self.head:
            self.head = new_node
            self.tail = new_node
        else:
            self.tail.next = new_node
            self.tail = new_node
        
        self._size += 1
    
    def prepend(self, data: T) -> None:
        """Add to beginning - O(1)"""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
        
        if not self.tail:
            self.tail = new_node
        
        self._size += 1
    
    def delete(self, data: T) -> bool:
        """Delete first occurrence - O(n)"""
        if not self.head:
            return False
        
        if self.head.data == data:
            self.head = self.head.next
            if not self.head:
                self.tail = None
            self._size -= 1
            return True
        
        current = self.head
        while current.next and current.next.data != data:
            current = current.next
        
        if not current.next:
            return False
        
        current.next = current.next.next
        if not current.next:
            self.tail = current
        self._size -= 1
        return True
    
    def reverse(self) -> None:
        """Reverse in place - O(n) time, O(1) space"""
        prev = None
        current = self.head
        self.tail = self.head
        
        while current:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        
        self.head = prev


# Usage
ll = LinkedList[int]()
ll.append(10)
ll.append(20)
ll.append(30)
print(ll)  # 10 -> 20 -> 30 -> None

ll.prepend(5)
print(ll)  # 5 -> 10 -> 20 -> 30 -> None

ll.reverse()
print(ll)  # 30 -> 20 -> 10 -> 5 -> None

for item in ll:
    print(item)  # 30, 20, 10, 5
```

### Common Interview Algorithms

```java
public class LinkedListAlgorithms {
    
    // Reverse a linked list - O(n) time, O(1) space
    public ListNode reverse(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        
        while (current != null) {
            ListNode next = current.next;  // Save next
            current.next = prev;            // Reverse pointer
            prev = current;                 // Move prev forward
            current = next;                 // Move current forward
        }
        
        return prev;  // New head
    }
    
    // Detect cycle (Floyd's Algorithm) - O(n) time, O(1) space
    public boolean hasCycle(ListNode head) {
        if (head == null) return false;
        
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;           // Move 1 step
            fast = fast.next.next;      // Move 2 steps
            
            if (slow == fast) {
                return true;  // Cycle detected!
            }
        }
        
        return false;  // Fast reached end, no cycle
    }
    
    // Find middle node - O(n) time, O(1) space
    public ListNode findMiddle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;  // Slow is at middle when fast reaches end
    }
    
    // Merge two sorted lists - O(n + m)
    public ListNode mergeSorted(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);  // Dummy head
        ListNode current = dummy;
        
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }
        
        // Attach remaining nodes
        current.next = (l1 != null) ? l1 : l2;
        
        return dummy.next;
    }
    
    // Remove nth node from end - O(n) single pass
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode fast = dummy;
        ListNode slow = dummy;
        
        // Move fast n+1 steps ahead
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }
        
        // Move both until fast reaches end
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        
        // Skip the nth node
        slow.next = slow.next.next;
        
        return dummy.next;
    }
    
    // Find intersection point of two lists
    public ListNode getIntersection(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;
        
        ListNode a = headA;
        ListNode b = headB;
        
        // When they meet, they've traveled same distance
        // If no intersection, both become null together
        while (a != b) {
            a = (a == null) ? headB : a.next;
            b = (b == null) ? headA : b.next;
        }
        
        return a;
    }
}
```

### Doubly Linked List (Python)

```python
class DoublyNode:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None


class DoublyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self._size = 0
    
    def append(self, data) -> None:
        """Add to end - O(1)"""
        new_node = DoublyNode(data)
        
        if not self.head:
            self.head = new_node
            self.tail = new_node
        else:
            new_node.prev = self.tail
            self.tail.next = new_node
            self.tail = new_node
        
        self._size += 1
    
    def prepend(self, data) -> None:
        """Add to beginning - O(1)"""
        new_node = DoublyNode(data)
        
        if not self.head:
            self.head = new_node
            self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node
        
        self._size += 1
    
    def delete_node(self, node: DoublyNode) -> None:
        """Delete specific node - O(1) if you have the node reference"""
        if node.prev:
            node.prev.next = node.next
        else:
            self.head = node.next
        
        if node.next:
            node.next.prev = node.prev
        else:
            self.tail = node.prev
        
        self._size -= 1
    
    def move_to_front(self, node: DoublyNode) -> None:
        """Move existing node to front - O(1)
        Useful for LRU cache implementation
        """
        if node == self.head:
            return
        
        # Remove from current position
        self.delete_node(node)
        self._size += 1  # Compensate for delete's decrement
        
        # Add to front
        node.prev = None
        node.next = self.head
        if self.head:
            self.head.prev = node
        self.head = node
        
        if not self.tail:
            self.tail = node
```

## Common Patterns

> [!tip] Dummy Head Node
> Using a dummy node simplifies edge cases for head modifications:
> ```java
> ListNode dummy = new ListNode(0);
> dummy.next = head;
> // ... modify list ...
> return dummy.next;  // Return actual head
> ```

> [!tip] Fast and Slow Pointers
> Essential technique for:
> - Finding middle element
> - Detecting cycles
> - Finding the start of a cycle
> ```java
> ListNode slow = head, fast = head;
> while (fast != null && fast.next != null) {
>     slow = slow.next;
>     fast = fast.next.next;
> }
> ```

> [!warning] Lost References
> When modifying `next` pointers, save references before changing:
> ```java
> // WRONG - loses reference to rest of list!
> current.next = prev;
> current = current.next;  // Now current is prev!
> 
> // CORRECT
> ListNode next = current.next;  // Save first
> current.next = prev;
> current = next;
> ```

## Edge Cases & Gotchas

- **Empty list** (`head == null`) — Always check before accessing
- **Single node** — `head.next` is null; head is also tail
- **Two nodes** — Middle finding behaves differently
- **Cycle detection** — Ensure you don't infinite loop
- **Memory leaks** — In languages without GC, explicitly null out removed nodes
- **Modifying during iteration** — Can cause unexpected behavior

## Related Topics

- [[Arrays]] — Contiguous alternative with O(1) access
- [[Stacks]] — Often implemented with linked lists
- [[Queues]] — Natural fit for linked list implementation
- [[Hash-Tables]] — Use linked lists for collision chaining
- [[LRU-Cache]] — Classic doubly linked list + hash map problem

## References

- [Visualgo - Linked List Visualization](https://visualgo.net/en/list)
- [GeeksforGeeks - Linked List](https://www.geeksforgeeks.org/data-structures/linked-list/)
- [LeetCode Linked List Problems](https://leetcode.com/tag/linked-list/)
