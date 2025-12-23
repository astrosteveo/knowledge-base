---
title: Hash Tables
category: Concepts/Data-Structures
tags:
  - data-structures
  - associative
  - interviews
  - fundamentals
difficulty: intermediate
status: evergreen
date-created: 2025-12-08
date-updated: 2025-12-08
sources:
  - https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html
  - https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
---

# Hash Tables

> [!summary]
> A hash table (also called hash map, dictionary, or associative array) stores key-value pairs with $O(1)$ average-case lookup, insertion, and deletion. It's arguably the most important data structure for practical programming—used everywhere from database indexing to caching to counting occurrences. If you learn one data structure well, make it this one.

## Theory

### What Is a Hash Table?

A hash table maps **keys** to **values** using a **hash function** to compute an index into an array of buckets.

```
Key "apple" → hash("apple") = 2 → store at index 2

Index:  0        1        2        3        4
      ┌────┐  ┌────┐  ┌────────────┐  ┌────┐  ┌────┐
      │    │  │    │  │apple → 5   │  │    │  │    │
      └────┘  └────┘  └────────────┘  └────┘  └────┘
```

**Key components:**
1. **Hash function** — Converts key to array index
2. **Buckets** — Array slots that store entries
3. **Collision handling** — Strategy when two keys hash to same index

### How Hashing Works

```python
def simple_hash(key: str, table_size: int) -> int:
    """Simple hash function example."""
    hash_value = 0
    for char in key:
        hash_value = (hash_value * 31 + ord(char)) % table_size
    return hash_value

# "apple" might hash to index 2
# "banana" might hash to index 7
# "apricot" might ALSO hash to index 2 → Collision!
```

### Collision Resolution

When two keys hash to the same index:

**1. Chaining (Separate Chaining)**
```
Index 2: [apple→5] → [apricot→3] → null
```
Each bucket holds a linked list of colliding entries.

**2. Open Addressing (Linear Probing)**
```
Index 2 full? Try index 3, then 4, etc.
```
Find the next empty slot.

### Time Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Insert | $O(1)$ | $O(n)$ |
| Lookup | $O(1)$ | $O(n)$ |
| Delete | $O(1)$ | $O(n)$ |

> [!info] Why O(n) Worst Case?
> If all keys hash to the same bucket (bad hash function or attack), the hash table degrades to a linked list. Good hash functions and load factor management prevent this.

### Load Factor

$$\text{Load Factor} = \frac{\text{Number of entries}}{\text{Number of buckets}}$$

- Load factor > 0.75 typically triggers **rehashing** (resize + rehash all entries)
- Trade-off: More buckets = fewer collisions but more memory

## Practical Examples

### Basic HashMap Usage (Java)

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> ages = new HashMap<>();

// Insert - O(1)
ages.put("Alice", 30);
ages.put("Bob", 25);
ages.put("Charlie", 35);

// Lookup - O(1)
int aliceAge = ages.get("Alice");  // 30
Integer unknownAge = ages.get("Unknown");  // null

// Check existence - O(1)
boolean hasAlice = ages.containsKey("Alice");  // true
boolean has30 = ages.containsValue(30);  // true (but O(n)!)

// Update
ages.put("Alice", 31);  // Overwrites previous value

// Remove - O(1)
ages.remove("Bob");

// Safe get with default
int age = ages.getOrDefault("Unknown", 0);  // 0

// Iterate
for (Map.Entry<String, Integer> entry : ages.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// Useful methods
ages.putIfAbsent("Diana", 28);  // Only adds if key doesn't exist
ages.computeIfAbsent("Eve", k -> k.length());  // Compute if absent
ages.merge("Alice", 1, Integer::sum);  // Increment Alice's age
```

### Python Dictionary

```python
# Python dicts are highly optimized hash tables

ages = {}

# Insert - O(1)
ages["Alice"] = 30
ages["Bob"] = 25
ages["Charlie"] = 35

# Lookup - O(1)
alice_age = ages["Alice"]  # 30
# ages["Unknown"]  # KeyError!

# Safe lookup
unknown_age = ages.get("Unknown")  # None
unknown_age = ages.get("Unknown", 0)  # 0 (default)

# Check existence - O(1)
has_alice = "Alice" in ages  # True

# Remove - O(1)
del ages["Bob"]
removed = ages.pop("Charlie", None)  # Remove and return

# Iterate
for name, age in ages.items():
    print(f"{name}: {age}")

# Dictionary comprehension
squares = {x: x**2 for x in range(10)}
# {0: 0, 1: 1, 2: 4, 3: 9, ...}

# Useful methods
ages.setdefault("Diana", 28)  # Set if key doesn't exist
ages.update({"Eve": 22, "Frank": 40})  # Merge another dict
```

### Common Pattern: Counting Occurrences

```java
// Count character frequency
public Map<Character, Integer> countChars(String s) {
    Map<Character, Integer> counts = new HashMap<>();
    
    for (char c : s.toCharArray()) {
        counts.merge(c, 1, Integer::sum);
        // Or: counts.put(c, counts.getOrDefault(c, 0) + 1);
    }
    
    return counts;
}

// Find most frequent element
public int mostFrequent(int[] nums) {
    Map<Integer, Integer> counts = new HashMap<>();
    int maxCount = 0;
    int result = nums[0];
    
    for (int num : nums) {
        int count = counts.merge(num, 1, Integer::sum);
        if (count > maxCount) {
            maxCount = count;
            result = num;
        }
    }
    
    return result;
}
```

```python
from collections import Counter

# Count with Counter (specialized dict)
text = "hello world"
char_counts = Counter(text)
# Counter({'l': 3, 'o': 2, 'h': 1, 'e': 1, ...})

# Most common
print(char_counts.most_common(2))  # [('l', 3), ('o', 2)]

# Manual counting
def count_chars(s: str) -> dict[str, int]:
    counts = {}
    for char in s:
        counts[char] = counts.get(char, 0) + 1
    return counts
```

### Two Sum (Classic Interview Problem)

```java
// Find two indices where nums[i] + nums[j] = target
public int[] twoSum(int[] nums, int target) {
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
// O(n) time, O(n) space - vs O(n²) brute force!
```

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    seen = {}  # value -> index
    
    for i, num in enumerate(nums):
        complement = target - num
        
        if complement in seen:
            return [seen[complement], i]
        
        seen[num] = i
    
    return []
```

### Group Anagrams

```python
from collections import defaultdict

def group_anagrams(strs: list[str]) -> list[list[str]]:
    """
    Group strings that are anagrams of each other.
    Input: ["eat","tea","tan","ate","nat","bat"]
    Output: [["eat","tea","ate"],["tan","nat"],["bat"]]
    """
    groups = defaultdict(list)
    
    for s in strs:
        # Sorted string as key - anagrams have same sorted form
        key = tuple(sorted(s))
        groups[key].append(s)
    
    return list(groups.values())


# Alternative: Use character count as key
def group_anagrams_v2(strs: list[str]) -> list[list[str]]:
    groups = defaultdict(list)
    
    for s in strs:
        # Count array as key (26 letters)
        count = [0] * 26
        for c in s:
            count[ord(c) - ord('a')] += 1
        
        groups[tuple(count)].append(s)
    
    return list(groups.values())
```

### LRU Cache (Hash Table + Doubly Linked List)

```python
from collections import OrderedDict

class LRUCache:
    """
    Least Recently Used cache.
    O(1) get and put operations.
    """
    
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = OrderedDict()  # Maintains insertion order
    
    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        
        # Move to end (most recently used)
        self.cache.move_to_end(key)
        return self.cache[key]
    
    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)
        
        self.cache[key] = value
        
        if len(self.cache) > self.capacity:
            # Remove least recently used (first item)
            self.cache.popitem(last=False)


# Usage
cache = LRUCache(2)
cache.put(1, 1)
cache.put(2, 2)
print(cache.get(1))  # 1 (moves 1 to end)
cache.put(3, 3)      # Evicts key 2
print(cache.get(2))  # -1 (not found)
```

### Hash Table Implementation (Educational)

```python
class HashTable:
    """
    Simple hash table with chaining for collision resolution.
    For understanding - use built-in dict in practice!
    """
    
    def __init__(self, capacity: int = 16):
        self.capacity = capacity
        self.size = 0
        self.buckets = [[] for _ in range(capacity)]
        self.load_factor_threshold = 0.75
    
    def _hash(self, key) -> int:
        """Compute bucket index for key."""
        return hash(key) % self.capacity
    
    def put(self, key, value) -> None:
        """Insert or update key-value pair."""
        if self.size / self.capacity >= self.load_factor_threshold:
            self._resize()
        
        index = self._hash(key)
        bucket = self.buckets[index]
        
        # Check if key exists, update if so
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)
                return
        
        # Key doesn't exist, append
        bucket.append((key, value))
        self.size += 1
    
    def get(self, key):
        """Retrieve value for key, or None if not found."""
        index = self._hash(key)
        bucket = self.buckets[index]
        
        for k, v in bucket:
            if k == key:
                return v
        
        return None
    
    def remove(self, key) -> bool:
        """Remove key-value pair. Returns True if found."""
        index = self._hash(key)
        bucket = self.buckets[index]
        
        for i, (k, v) in enumerate(bucket):
            if k == key:
                del bucket[i]
                self.size -= 1
                return True
        
        return False
    
    def _resize(self) -> None:
        """Double capacity and rehash all entries."""
        old_buckets = self.buckets
        self.capacity *= 2
        self.buckets = [[] for _ in range(self.capacity)]
        self.size = 0
        
        for bucket in old_buckets:
            for key, value in bucket:
                self.put(key, value)
```

## Common Patterns

> [!tip] Hash Table as Cache
> Store computed results to avoid recomputation:
> ```python
> cache = {}
> def expensive_function(x):
>     if x not in cache:
>         cache[x] = compute(x)  # Only compute once
>     return cache[x]
> ```

> [!tip] Default Values with defaultdict
> ```python
> from collections import defaultdict
> 
> # Instead of checking if key exists
> graph = defaultdict(list)
> graph["A"].append("B")  # No KeyError!
> 
> counts = defaultdict(int)
> counts["x"] += 1  # Defaults to 0, then increments
> ```

> [!warning] Mutable Keys
> Keys must be **hashable** (immutable in Python):
> - OK: `str`, `int`, `tuple`, `frozenset`
> - NOT OK: `list`, `dict`, `set`
> 
> ```python
> d = {}
> d[[1, 2]] = "value"  # TypeError: unhashable type: 'list'
> d[(1, 2)] = "value"  # OK - tuple is hashable
> ```

> [!warning] Hash Collisions in Interviews
> When asked about hash table complexity, always mention:
> - Average: $O(1)$
> - Worst: $O(n)$ with bad hash function or many collisions

## Edge Cases & Gotchas

- **Null keys** — Java HashMap allows one null key; some don't
- **Key equality** — Keys are compared using `equals()` / `==`, not identity
- **Iteration order** — Regular HashMap has no guaranteed order (use LinkedHashMap/OrderedDict for insertion order)
- **Thread safety** — HashMap/dict are not thread-safe (use ConcurrentHashMap or locks)
- **Hash function quality** — Poor hash functions cause clustering and degrade to $O(n)$

## Related Topics

- [[Sets]] — Hash tables storing only keys (no values)
- [[Arrays]] — Underlying storage for buckets
- [[Linked-Lists]] — Used for chaining collision resolution
- [[Trees]] — Java 8+ HashMap uses trees for long chains
- [[LRU-Cache]] — Classic hash table + linked list combination

## References

- [Java HashMap Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)
- [Python dict Implementation](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)
- [Hash Table Visualization](https://www.cs.usfca.edu/~galles/visualization/OpenHash.html)
