---
title: Documentation Standards
date-created: 2025-12-08
date-updated: 2025-12-08
---

# Documentation Standards

> [!summary]
> This document defines the structure, format, and conventions for all technical documentation in this Obsidian vault. These standards ensure consistency and optimize for learning.

## Vault Structure

```
knowledge-base/
├── Languages/           # Core language features
│   ├── Java/
│   ├── Python/
│   └── [language]/
├── Frameworks/          # Framework-specific concepts
│   ├── React/
│   ├── Spring/
│   └── [framework]/
├── Concepts/            # Cross-cutting ideas
│   ├── Design-Patterns/
│   ├── Data-Structures/
│   └── Algorithms/
└── Meta/                # Vault documentation
```

### Category Logic

| Category | Contains | Examples |
|----------|----------|----------|
| Languages/ | Core language features | Java Annotations, Python Decorators, TypeScript Generics |
| Frameworks/ | Framework-specific concepts | React Hooks, Spring Beans, Django ORM |
| Concepts/ | Cross-cutting ideas | Design Patterns, Data Structures, Algorithms |

## Document Template

Every documentation file follows this structure:

### Frontmatter (Required)

```yaml
---
title: Topic Name
category: Languages/Java
tags:
  - java
  - feature-type
difficulty: beginner|intermediate|advanced
date-created: YYYY-MM-DD
date-updated: YYYY-MM-DD
sources:
  - https://official-docs-url
---
```

### Content Sections

1. **Summary callout** — One paragraph overview
2. **Theory** — What it is, why it matters, how it works
3. **Practical Examples** — Basic, Intermediate, Advanced (minimum 3)
4. **Common Patterns** — Best practices with `> [!tip]` callouts
5. **Edge Cases & Gotchas** — Things that trip people up
6. **Related Topics** — `[[wiki-links]]` to connected concepts
7. **References** — Authoritative external sources

## Formatting Conventions

| Element        | Format                        |
| -------------- | ----------------------------- |
| Code blocks    | Specify language: ` ```java ` |
| Tips           | `> [!tip] Title`              |
| Warnings       | `> [!warning] Title`          |
| Info           | `> [!info] Title`             |
| Summary        | `> [!summary]`                |
| Internal links | `[[Title-Case-With-Hyphens]]` |
| Math notation  | LaTeX: `$O(n \log n)$`        |
| Diagrams       | Mermaid code blocks           |

## File Naming

- Use Title-Case with hyphens: `Java-Annotations.md`
- Index files: `_Index.md` (underscore prefix sorts first)
- No spaces in filenames

## Index Files

Each folder has an `_Index.md` that:
- Provides overview of the category
- Links to all documents in the folder
- Groups topics logically (Core, Advanced, etc.)
- Links to related categories

## Quality Standards

- All code examples must be runnable (not pseudocode)
- Minimum 3 examples per document (basic → advanced)
- At least one `> [!tip]` and one `> [!warning]` per document
- Always include authoritative sources in References
- Update `_Index.md` when adding new documents
