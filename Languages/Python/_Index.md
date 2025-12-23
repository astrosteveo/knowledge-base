---
title: Python Index
date-updated: 2025-12-10
---

# Python

> [!summary]
> Python is a high-level, interpreted programming language known for its clear syntax, readability, and versatility. This section covers core Python language features, idioms, and advanced concepts.

## Recently Updated

```dataview
TABLE difficulty, status, file.mtime as "Modified"
FROM "Languages/Python"
WHERE file.name != "_Index"
SORT file.mtime DESC
LIMIT 5
```

## By Status

### 🌱 Seeds (need expansion)
```dataview
LIST FROM "Languages/Python" WHERE status = "seed" AND file.name != "_Index"
```

### 🌿 Growing
```dataview
LIST FROM "Languages/Python" WHERE status = "growing" AND file.name != "_Index"
```

### 🌳 Evergreen (comprehensive)
```dataview
LIST FROM "Languages/Python" WHERE status = "evergreen" AND file.name != "_Index"
```

## All Topics

```dataview
TABLE difficulty, status
FROM "Languages/Python"
WHERE file.name != "_Index"
SORT file.name ASC
```

## See Also

- [[../Java/_Index|Java]] — Another popular object-oriented language
- [[../../Concepts/Design-Patterns/_Index|Design Patterns]] — Language-agnostic design patterns
