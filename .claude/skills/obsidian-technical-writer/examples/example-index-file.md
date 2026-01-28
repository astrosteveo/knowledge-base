# Example Index File

This is a complete example index file following all conventions. Use this as a reference when creating or updating category index files.

---

````markdown
---
title: Data Structures Index
category: Concepts/Data-Structures
tags:
  - index
  - data-structures
  - algorithms
date-created: 2025-12-01
date-updated: 2026-01-13
---

# Data Structures

> [!summary]
> Data structures organize and store data efficiently for specific operations. Choosing the right structure — arrays for random access, linked lists for insertions, hash tables for lookups, trees for hierarchical data — dramatically impacts performance. Master these fundamentals to write efficient, scalable code.

## Overview

Data structures are the foundation of efficient algorithms. Each structure offers trade-offs between different operations:

- **Arrays** provide O(1) access but expensive insertions
- **Linked lists** enable O(1) insertions but slow lookups
- **Hash tables** deliver O(1) average-case operations with memory overhead
- **Trees** balance multiple operations with O(log n) complexity
- **Graphs** model relationships and networks

This section covers both fundamental structures (arrays, stacks, queues) and advanced structures (B-trees, tries, heaps) with implementation details, complexity analysis, and practical use cases.

## Learning Path

### Beginner

Start with fundamental linear structures:

- [[Arrays]] — Contiguous memory storage with O(1) access
- [[Linked-Lists]] — Node-based structure with O(1) insertion
- [[Stacks]] — Last-In-First-Out (LIFO) operations
- [[Queues]] — First-In-First-Out (FIFO) operations
- [[Hash-Tables]] — Key-value storage with O(1) average lookup

### Intermediate

Progress to hierarchical and specialized structures:

- [[Binary-Search-Trees]] — Ordered tree structure with O(log n) operations
- [[Heaps]] — Priority queue implementation
- [[Tries]] — Prefix tree for string operations
- [[Graphs]] — Vertices and edges modeling relationships
- [[Sets]] — Unique element collections

### Advanced

Master complex and optimized structures:

- [[AVL-Trees]] — Self-balancing binary search tree
- [[Red-Black-Trees]] — Another balanced tree with relaxed rules
- [[B-Trees]] — Multi-way trees for databases and filesystems
- [[Skip-Lists]] — Probabilistic balanced structure
- [[Bloom-Filters]] — Space-efficient probabilistic set membership

## Topics

### Linear Structures

| Topic | Difficulty | Status | Description |
|-------|-----------|--------|-------------|
| [[Arrays]] | Beginner | 🌳 Evergreen | Contiguous memory storage with O(1) indexing |
| [[Linked-Lists]] | Beginner | 🌳 Evergreen | Node-based list with O(1) insertion/deletion |
| [[Stacks]] | Beginner | 🌳 Evergreen | LIFO structure using arrays or lists |
| [[Queues]] | Beginner | 🌳 Evergreen | FIFO structure with various implementations |
| [[Deques]] | Intermediate | 🌿 Growing | Double-ended queue supporting both ends |

### Hash-Based Structures

| Topic | Difficulty | Status | Description |
|-------|-----------|--------|-------------|
| [[Hash-Tables]] | Beginner | 🌳 Evergreen | Key-value storage with O(1) average operations |
| [[Hash-Sets]] | Beginner | 🌳 Evergreen | Unique element storage using hashing |
| [[Hash-Functions]] | Intermediate | 🌿 Growing | Designing effective hash functions |
| [[Collision-Resolution]] | Intermediate | 🌿 Growing | Handling hash collisions (chaining, open addressing) |

### Tree Structures

| Topic | Difficulty | Status | Description |
|-------|-----------|--------|-------------|
| [[Binary-Trees]] | Intermediate | 🌳 Evergreen | Hierarchical structure with max 2 children per node |
| [[Binary-Search-Trees]] | Intermediate | 🌳 Evergreen | Ordered tree with left < parent < right property |
| [[AVL-Trees]] | Advanced | 🌳 Evergreen | Self-balancing BST with height balance |
| [[Red-Black-Trees]] | Advanced | 🌿 Growing | Self-balancing BST with color properties |
| [[B-Trees]] | Advanced | 🌿 Growing | Multi-way trees optimized for disk access |
| [[Tries]] | Intermediate | 🌿 Growing | Prefix tree for string operations |
| [[Heaps]] | Intermediate | 🌳 Evergreen | Complete binary tree for priority queues |

### Graph Structures

| Topic | Difficulty | Status | Description |
|-------|-----------|--------|-------------|
| [[Graphs]] | Intermediate | 🌳 Evergreen | Vertices and edges modeling relationships |
| [[Adjacency-Matrix]] | Intermediate | 🌳 Evergreen | 2D array graph representation |
| [[Adjacency-List]] | Intermediate | 🌳 Evergreen | Array/map of lists for graph representation |
| [[Directed-Graphs]] | Intermediate | 🌿 Growing | Graphs with directed edges |
| [[Weighted-Graphs]] | Intermediate | 🌿 Growing | Graphs with edge weights |

### Specialized Structures

| Topic | Difficulty | Status | Description |
|-------|-----------|--------|-------------|
| [[Bloom-Filters]] | Advanced | 🌱 Seed | Probabilistic set membership testing |
| [[Skip-Lists]] | Advanced | 🌱 Seed | Probabilistic balanced structure alternative to trees |
| [[Disjoint-Sets]] | Advanced | 🌿 Growing | Union-find data structure for connectivity |
| [[Segment-Trees]] | Advanced | 🌱 Seed | Tree for range query operations |
| [[Fenwick-Trees]] | Advanced | 🌱 Seed | Binary indexed tree for prefix sums |

## Recently Updated

```dataview
TABLE date-updated as "Last Updated", difficulty as "Difficulty", status as "Status"
FROM "Concepts/Data-Structures"
WHERE file.name != "_Index"
SORT date-updated DESC
LIMIT 5
```

## Quick Reference

### Time Complexity Comparison

| Structure | Access | Search | Insert | Delete | Space |
|-----------|--------|--------|--------|--------|-------|
| Array | O(1) | O(n) | O(n) | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1)\* | O(1)\* | O(n) |
| Hash Table | N/A | O(1)\*\* | O(1)\*\* | O(1)\*\* | O(n) |
| BST | O(log n)\*\*\* | O(log n)\*\*\* | O(log n)\*\*\* | O(log n)\*\*\* | O(n) |
| AVL Tree | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| Heap | N/A | O(n) | O(log n) | O(log n) | O(n) |
| Trie | O(k) | O(k) | O(k) | O(k) | O(n×k) |

\* At known position
\*\* Average case (worst case O(n) with collisions)
\*\*\* For balanced trees; O(n) worst case for unbalanced
k = key length for tries

### When to Use Each Structure

| Use Case | Recommended Structure | Reason |
|----------|----------------------|--------|
| Random access by index | Array | O(1) indexing |
| Frequent insertions/deletions | Linked List | O(1) at known position |
| Fast lookups by key | Hash Table | O(1) average search |
| Maintain sorted order | BST/AVL Tree | O(log n) operations + sorted traversal |
| Priority-based processing | Heap | O(log n) extract-min/max |
| Prefix matching (autocomplete) | Trie | O(k) search by prefix |
| Relationships/networks | Graph | Models connections naturally |
| Range queries | Segment Tree | O(log n) range operations |

### Implementation Languages

Each data structure document provides implementations in:
- **Java** — Using built-in collections and custom implementations
- **Python** — Using standard library and custom classes
- **Pseudocode** — Language-agnostic algorithm descriptions

## Study Resources

### Essential Reading

- Cormen, et al. *Introduction to Algorithms* (CLRS), 4th ed.
  - Chapter 10-13: Elementary data structures
  - Chapter 18: B-Trees
  - Chapter 20-22: Advanced data structures

- Skiena, Steven. *The Algorithm Design Manual*, 3rd ed.
  - Part I: Data structures catalog

### Online Visualizations

- [VisuAlgo](https://visualgo.net/) — Animated data structure operations
- [Data Structure Visualizations](https://www.cs.usfca.edu/~galles/visualization/) — Interactive Java applets

### Practice Problems

- [LeetCode Data Structure Track](https://leetcode.com/problemset/) — Filter by data structure tag
- [HackerRank Data Structures](https://www.hackerrank.com/domains/data-structures) — Structured practice

## Related Categories

- [[Concepts/Algorithms/_Index]] — Algorithms that operate on data structures
- [[Languages/Java/_Index]] — Java implementations and collections framework
- [[Languages/Python/_Index]] — Python data structures and collections module
- [[Concepts/Design-Patterns/_Index]] — Structural patterns using data structures

## Notes

**Content Status Legend:**
- 🌱 **Seed** — Basic outline, under development
- 🌿 **Growing** — Substantial content, still expanding
- 🌳 **Evergreen** — Complete, comprehensive, regularly updated

**Contribution Guidelines:**
1. Follow the standard document template from [[Meta/Templates/Technical-Document]]
2. Include minimum 3 code examples (basic, intermediate, advanced)
3. Provide time/space complexity analysis
4. Show both good and bad usage patterns
5. Add Mermaid diagrams for structure visualization
6. Cross-reference related data structures and algorithms

---

**Maintainer:** Knowledge base owner
**Last Major Update:** 2026-01-13
**Next Review:** 2026-04-13
````

---

## What This Example Demonstrates

**Frontmatter:**
- Index-specific structure
- Appropriate tags including "index"
- Creation and update dates tracked

**Index Structure:**
1. **Summary callout** — Overview of the category and its value
2. **Overview** — 2-3 paragraphs of context
3. **Learning Path** — Organized by difficulty with descriptions
4. **Topics** — Multiple tables grouped by subcategory
5. **Recently Updated** — Dataview query showing latest changes
6. **Quick Reference** — Category-specific reference material (comparison tables)
7. **Study Resources** — External references for deeper learning
8. **Related Categories** — Cross-links to other indexes

**Organization:**
- Topics grouped logically (Linear, Hash-Based, Trees, Graphs, Specialized)
- Each topic has difficulty, status emoji, and description
- Clear progression from beginner → intermediate → advanced

**Quick Reference Tables:**
- Time complexity comparison across structures
- "When to Use" guide matching use cases to structures
- Implementation language coverage

**Visual Organization:**
- Status emojis for quick scanning (🌱🌿🌳)
- Tables for structured information
- Bullet points for lists
- Clear section headings

**Dataview Integration:**
- Query to show recently updated documents
- Sorted by date with relevant metadata
- Filters out the index file itself

**Study Support:**
- External resources (books, websites)
- Practice problem links
- Visualization tools

**Maintainability:**
- Contribution guidelines for consistency
- Maintainer and review schedule noted
- Content status legend explained

This index file serves as both navigation and learning guide, helping users understand the category structure and choose where to start based on their level.