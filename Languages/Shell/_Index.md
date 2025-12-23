---
title: Shell Index
date-updated: 2025-12-10
---

# Shell / Command Line

> [!summary]
> Essential command-line tools and shell scripting for Unix/Linux environments. These tools form the foundation of text processing, automation, and system administration.

## Recently Updated

```dataview
TABLE difficulty, status, file.mtime as "Modified"
FROM "Languages/Shell"
WHERE file.name != "_Index"
SORT file.mtime DESC
LIMIT 5
```

## By Status

### 🌱 Seeds (need expansion)
```dataview
LIST FROM "Languages/Shell" WHERE status = "seed" AND file.name != "_Index"
```

### 🌿 Growing
```dataview
LIST FROM "Languages/Shell" WHERE status = "growing" AND file.name != "_Index"
```

### 🌳 Evergreen (comprehensive)
```dataview
LIST FROM "Languages/Shell" WHERE status = "evergreen" AND file.name != "_Index"
```

## All Topics

```dataview
TABLE difficulty, status
FROM "Languages/Shell"
WHERE file.name != "_Index"
SORT file.name ASC
```

## Tool Categories

### Text Processing
- [[grep]] — Search text using patterns
- [[sed]] — Stream editor for transformations
- [[awk]] — Pattern scanning and processing

### Pipeline Utilities
- [[xargs]] — Build and execute commands from input
- [[fzf]] — Fuzzy finder for interactive selection

## See Also

- [[../Python/_Index|Python]] — Often used alongside shell for automation
- Bash scripting fundamentals *(coming soon)*
- Regular expressions *(coming soon)*
