---
title: Queues
category: Concepts/Data-Structures
tags:
  - data-structures
  - linear
  - FIFO
  - fundamentals
difficulty: beginner
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html
  - https://www.geeksforgeeks.org/queue-data-structure/
---

# Queues

> [!summary]
> A queue is a First-In-First-Out (FIFO) data structure where elements are added at the rear and removed from the front. Like a line at a store—first person in line is first served. Queues are essential for scheduling, breadth-first search, buffering, and any scenario requiring fair ordering.

## Theory

### What Is a Queue?

A queue restricts access through operations at opposite ends:
- **Enqueue** — Add element to the rear (back)
- **Dequeue** — Remove element from the front

```
Enqueue 10, 20, 30:

Front                          Rear
  ↓                              ↓
┌────┬────┬────┐
│ 10 │ 20 │ 30 │  ← Enqueue here
└────┴────┴────┘
  ↑
Dequeue here (returns 10 first)
```

**FIFO Principle:** First element enqueued is first dequeued.

### Queue Variants

| Type | Description | Use Case |
|------|-------------|----------|
| **Simple Queue** | Basic FIFO | Task scheduling |
| **Circular Queue** | End wraps to beginning | Fixed-size buffers |
| **Priority Queue** | Dequeue by priority, not order | Dijkstra's algorithm, task prioritization |
| **Deque** | Insert/remove at both ends | Sliding window problems |

### Core Operations

| Operation | Description | Time |
|-----------|-------------|------|
| `enqueue(x)` / `offer(x)` | Add x to rear | $O(1)$ |
| `dequeue()` / `poll()` | Remove from front | $O(1)$ |
| `peek()` / `front()` | View front without removing | $O(1)$ |
| `isEmpty()` | Check if queue is empty | $O(1)$ |
| `size()` | Number of elements | $O(1)$ |

### Real-World Uses

- **CPU scheduling** — Process jobs in order received
- **Print queue** — Documents print in submission order
- **BFS traversal** — Explore graph level by level
- **Message queues** — Kafka, RabbitMQ, SQS
- **Buffering** — IO operations, video streaming
- **Call center systems** — First caller answered first

## Practical Examples

### Queue Implementation (Java)

```java
public class Queue<T> {
    private static class Node<T> {
        T data;
        Node<T> next;
        
        Node(T data) {
            this.data = data;
        }
    }
    
    private Node<T> front;
    private Node<T> rear;
    private int size;
    
    public void enqueue(T data) {
        Node<T> newNode = new Node<>(data);
        
        if (rear != null) {
            rear.next = newNode;
        }
        rear = newNode;
        
        if (front == null) {
            front = rear;
        }
        size++;
    }
    
    public T dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        T data = front.data;
        front = front.next;
        
        if (front == null) {
            rear = null;  // Queue is now empty
        }
        size--;
        return data;
    }
    
    public T peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return front.data;
    }
    
    public boolean isEmpty() {
        return front == null;
    }
    
    public int size() {
        return size;
    }
}
```

### Using Built-in Queue (Java)

```java
import java.util.LinkedList;
import java.util.Queue;

Queue<Integer> queue = new LinkedList<>();

// Enqueue
queue.offer(10);  // Returns true if added
queue.offer(20);
queue.offer(30);

// Peek (doesn't remove)
System.out.println(queue.peek());  // 10

// Dequeue
while (!queue.isEmpty()) {
    System.out.println(queue.poll());  // 10, 20, 30
}

// poll() returns null if empty (vs remove() throws exception)
System.out.println(queue.poll());  // null
```

### Python Queue with collections.deque

```python
from collections import deque

# deque is optimized for both ends - O(1) operations
queue = deque()

# Enqueue (append to right)
queue.append(10)
queue.append(20)
queue.append(30)

# Peek
front = queue[0]  # 10

# Dequeue (pop from left)
print(queue.popleft())  # 10
print(queue.popleft())  # 20
print(queue.popleft())  # 30

# Check empty
if not queue:
    print("Queue is empty")
```

### BFS with Queue (Classic Application)

```java
// Level-order traversal of binary tree
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> currentLevel = new ArrayList<>();
        
        // Process all nodes at current level
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            currentLevel.add(node.val);
            
            // Add children for next level
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        
        result.add(currentLevel);
    }
    
    return result;
}
```

```python
from collections import deque

def level_order(root):
    """BFS level-order traversal using queue."""
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        current_level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(current_level)
    
    return result
```

### Circular Queue (Ring Buffer)

```python
class CircularQueue:
    """
    Fixed-size queue using circular array.
    Useful for memory-efficient buffers.
    """
    
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.data = [None] * capacity
        self.front = 0
        self.rear = -1
        self.size = 0
    
    def enqueue(self, value) -> bool:
        if self.is_full():
            return False
        
        self.rear = (self.rear + 1) % self.capacity
        self.data[self.rear] = value
        self.size += 1
        return True
    
    def dequeue(self):
        if self.is_empty():
            return None
        
        value = self.data[self.front]
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return value
    
    def peek(self):
        if self.is_empty():
            return None
        return self.data[self.front]
    
    def is_empty(self) -> bool:
        return self.size == 0
    
    def is_full(self) -> bool:
        return self.size == self.capacity


# Usage
buffer = CircularQueue(3)
buffer.enqueue(1)
buffer.enqueue(2)
buffer.enqueue(3)
print(buffer.enqueue(4))  # False - full!
print(buffer.dequeue())   # 1
print(buffer.enqueue(4))  # True - now has space
```

### Priority Queue

```java
import java.util.PriorityQueue;

// Min-heap by default (smallest first)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(30);
minHeap.offer(10);
minHeap.offer(20);

while (!minHeap.isEmpty()) {
    System.out.println(minHeap.poll());  // 10, 20, 30
}

// Max-heap (largest first)
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
maxHeap.offer(30);
maxHeap.offer(10);
maxHeap.offer(20);

while (!maxHeap.isEmpty()) {
    System.out.println(maxHeap.poll());  // 30, 20, 10
}

// Custom priority (by task priority)
record Task(String name, int priority) {}

PriorityQueue<Task> taskQueue = new PriorityQueue<>(
    Comparator.comparingInt(Task::priority)
);
taskQueue.offer(new Task("Low priority", 3));
taskQueue.offer(new Task("High priority", 1));
taskQueue.offer(new Task("Medium priority", 2));

while (!taskQueue.isEmpty()) {
    System.out.println(taskQueue.poll().name());
    // High priority, Medium priority, Low priority
}
```

```python
import heapq

# Python heapq is a min-heap
heap = []
heapq.heappush(heap, 30)
heapq.heappush(heap, 10)
heapq.heappush(heap, 20)

while heap:
    print(heapq.heappop(heap))  # 10, 20, 30

# Max-heap (negate values)
max_heap = []
for val in [30, 10, 20]:
    heapq.heappush(max_heap, -val)

while max_heap:
    print(-heapq.heappop(max_heap))  # 30, 20, 10

# With priority tuples (priority, data)
task_queue = []
heapq.heappush(task_queue, (3, "Low priority"))
heapq.heappush(task_queue, (1, "High priority"))
heapq.heappush(task_queue, (2, "Medium priority"))

while task_queue:
    priority, task = heapq.heappop(task_queue)
    print(task)  # High priority, Medium priority, Low priority
```

### Sliding Window Maximum (Deque)

```python
from collections import deque

def max_sliding_window(nums: list[int], k: int) -> list[int]:
    """
    Find maximum in each sliding window of size k.
    Uses monotonic decreasing deque.
    
    Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
    Output: [3,3,5,5,6,7]
    """
    result = []
    dq = deque()  # Store indices, values are decreasing
    
    for i, num in enumerate(nums):
        # Remove elements outside window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        
        # Remove smaller elements (they'll never be max)
        while dq and nums[dq[-1]] < num:
            dq.pop()
        
        dq.append(i)
        
        # Window is full, record maximum
        if i >= k - 1:
            result.append(nums[dq[0]])
    
    return result
```

## Common Patterns

> [!tip] BFS = Queue, DFS = Stack
> - **Breadth-First Search**: Use queue to explore level by level
> - **Depth-First Search**: Use stack (or recursion) to go deep first
> 
> This is the key distinction for graph traversal!

> [!tip] Queue for "First K" Problems
> When you need to process items in order or find "first K" of something, queue is often the answer.

> [!warning] Queue vs Stack Choice
> - Need to process in order received? → Queue (FIFO)
> - Need to reverse or backtrack? → Stack (LIFO)
> - Need both? → Deque (double-ended)

## Edge Cases & Gotchas

- **Empty queue** — Check before `dequeue()` or `peek()`
- **Full queue** — Circular queues have capacity limits
- **Single element** — Front and rear point to same element
- **Blocking queues** — In concurrent programming, may wait for elements

## Related Topics

- [[Stacks]] — LIFO counterpart
- [[BFS]] — Breadth-first search relies on queues
- [[Heaps]] — Implement priority queues
- [[Linked-Lists]] — Common implementation for queues
- [[Message-Queues]] — Distributed system queues (Kafka, RabbitMQ)

## References

- [GeeksforGeeks - Queue](https://www.geeksforgeeks.org/queue-data-structure/)
- [Visualgo - Queue Visualization](https://visualgo.net/en/list)
- [Java Queue Interface](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)
