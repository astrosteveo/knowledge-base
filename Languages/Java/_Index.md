---
title: Java Index
date-updated: 2025-12-10
---

# Java

> [!summary]
> Documentation for Java language features, patterns, and best practices. Java is a statically-typed, object-oriented language known for its "write once, run anywhere" philosophy and extensive ecosystem.

## Recently Updated

```dataview
TABLE difficulty, status, file.mtime as "Modified"
FROM "Languages/Java"
WHERE file.name != "_Index"
SORT file.mtime DESC
LIMIT 5
```

## By Status

### 🌱 Seeds (need expansion)
```dataview
LIST FROM "Languages/Java" WHERE status = "seed" AND file.name != "_Index"
```

### 🌿 Growing
```dataview
LIST FROM "Languages/Java" WHERE status = "growing" AND file.name != "_Index"
```

### 🌳 Evergreen (comprehensive)
```dataview
LIST FROM "Languages/Java" WHERE status = "evergreen" AND file.name != "_Index"
```

## All Topics

```dataview
TABLE difficulty, status
FROM "Languages/Java"
WHERE file.name != "_Index"
SORT file.name ASC
```

---

## Learning Paths

### 🟢 Fundamentals (Start Here)

Core building blocks of Java — understand these before anything else:

| Order | Topic | What You'll Learn |
|-------|-------|-------------------|
| 1 | [[Primitives-and-Wrappers]] | `int` vs `Integer`, autoboxing, type conversion |
| 2 | [[Strings]] | String methods, StringBuilder, immutability |
| 3 | [[Classes-and-Objects]] | Classes, objects, fields, constructors, `this`, `static` |
| 4 | [[Methods]] | Parameters, return types, overloading, varargs, recursion |
| 5 | [[Inheritance-and-Polymorphism]] | `extends`, `super`, overriding, abstract classes |
| 6 | [[Interfaces]] | Contracts, `implements`, default methods, polymorphism |
| 7 | [[Enums]] | Constants, enum fields/methods, type-safe values |
| 8 | [[Collections-Framework]] | Lists, Sets, Maps, when to use what |
| 9 | [[Comparators-and-Comparable]] | Sorting custom objects, natural vs custom order |
| 10 | [[Exception-Handling]] | try-catch, checked vs unchecked, best practices |

### 🟡 Intermediate (Modern Java)

Features that make Java expressive and powerful:

| Order | Topic | What You'll Learn |
|-------|-------|-------------------|
| 1 | [[Generics]] | Type parameters, bounds, wildcards, PECS |
| 2 | [[Annotations]] | Metadata, custom annotations, reflection |
| 3 | [[Functional-Interfaces-and-Lambdas]] | `Function`, `Predicate`, method references |
| 4 | [[Streams-API]] | `filter`, `map`, `collect`, parallel streams |
| 5 | [[Optional]] | Null-safety, `orElse`, `map`, `flatMap` |

### 🔴 Advanced (Coming Soon)

- [[Concurrency-Basics]] — Threads, ExecutorService, synchronization
- [[Records-and-Sealed-Classes]] — Modern data carriers
- [[Reflection]] — Runtime class inspection
- [[JVM-Internals]] — Memory model, garbage collection

---

## Quick Navigation by Category

### Basics & Types
- [[Primitives-and-Wrappers]] — `int` vs `Integer`, autoboxing
- [[Strings]] — The most common type, StringBuilder

### Object-Oriented Programming
- [[Classes-and-Objects]] — The foundation of Java
- [[Methods]] — Functions that do things
- [[Inheritance-and-Polymorphism]] — Code reuse, `extends`, overriding
- [[Interfaces]] — Contracts and polymorphism
- [[Enums]] — Type-safe constants

### Data & Collections
- [[Collections-Framework]] — Lists, Sets, Maps, Queues
- [[Comparators-and-Comparable]] — Sorting custom objects
- [[Generics]] — Type-safe containers
- [[Streams-API]] — Declarative data processing

### Functional Programming
- [[Functional-Interfaces-and-Lambdas]] — Functions as values
- [[Streams-API]] — Pipeline processing
- [[Optional]] — Safe null handling

### Error Handling
- [[Exception-Handling]] — try-catch, throwing, best practices

### Metadata & Reflection
- [[Annotations]] — Decorating code with metadata

---

## See Also

- [[../../Frameworks/Spring/_Index|Spring Framework]] — Popular Java framework
- [[../../Concepts/Design-Patterns/_Index|Design Patterns]] — Object-oriented patterns
- [[../../Concepts/Data-Structures/_Index|Data Structures]] — Underlying concepts
