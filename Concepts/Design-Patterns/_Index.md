---
title: Design Patterns Index
date-updated: 2025-12-08
---

# Design Patterns

> [!summary]
> Design patterns are reusable solutions to common software design problems. Originally catalogued by the "Gang of Four" (Gamma, Helm, Johnson, Vlissides) in 1994, these patterns remain foundational knowledge for software engineers, providing a shared vocabulary and proven approaches to object-oriented design.

## Pattern Categories

Design patterns are organized into three categories based on their purpose:

| Category | Purpose | Patterns |
|----------|---------|----------|
| **Creational** | Object creation mechanisms | Singleton, Factory, Builder, Prototype, Abstract Factory |
| **Structural** | Object composition | Adapter, Decorator, Facade, Proxy, Composite, Bridge, Flyweight |
| **Behavioral** | Object interaction | Observer, Strategy, Command, State, Template Method, Iterator, Mediator, Chain of Responsibility, Visitor, Memento |

## Creational Patterns

- [[Singleton]] - Ensure a class has only one instance with global access
- [[Factory]] - Create objects without specifying exact classes
- [[Builder]] - Construct complex objects step by step
- [[Prototype]] - Clone existing objects
- [[Abstract-Factory]] - Create families of related objects

## Structural Patterns

- [[Adapter]] - Make incompatible interfaces work together
- [[Decorator]] - Add behavior to objects dynamically
- [[Facade]] - Simplify complex subsystem interfaces
- [[Proxy]] - Control access to objects
- [[Composite]] - Treat individual objects and compositions uniformly

## Behavioral Patterns

- [[Observer]] - Define one-to-many dependency between objects
- [[Strategy]] - Define interchangeable algorithms
- [[Command]] - Encapsulate requests as objects
- [[State]] - Alter behavior when internal state changes
- [[Template-Method]] - Define algorithm skeleton, defer steps to subclasses

## When to Use Patterns

> [!tip] Pattern Selection
> Don't force patterns into your code. Recognize the problem first, then apply the appropriate pattern. Overusing patterns leads to unnecessary complexity.

> [!warning] Anti-Pattern Alert
> Using patterns for the sake of using patterns is itself an anti-pattern. Keep solutions as simple as possible.

## See Also

- [[Data-Structures/_Index]] - Fundamental data organization
- [[Algorithms/_Index]] - Problem-solving procedures
- [[SOLID-Principles]] - Design principles that patterns often implement

## References

- [Gang of Four Design Patterns - DigitalOcean](https://www.digitalocean.com/community/tutorials/gangs-of-four-gof-design-patterns)
- [Design Patterns: Elements of Reusable Object-Oriented Software](https://en.wikipedia.org/wiki/Design_Patterns) (Original GoF Book)
- [Refactoring Guru - Design Patterns](https://refactoring.guru/design-patterns)


---

## By Status

### 🌱 Seeds (need expansion)
```dataview
LIST FROM "Concepts/Design-Patterns" WHERE status = "seed" AND file.name != "_Index"
```

### 🌿 Growing
```dataview
LIST FROM "Concepts/Design-Patterns" WHERE status = "growing" AND file.name != "_Index"
```

### 🌳 Evergreen (comprehensive)
```dataview
LIST FROM "Concepts/Design-Patterns" WHERE status = "evergreen" AND file.name != "_Index"
```

---

## 📊 All Patterns (Auto-Generated)

```dataview
TABLE 
    difficulty AS "Level",
    dateformat(date-updated, "yyyy-MM-dd") AS "Updated"
FROM "Concepts/Design-Patterns"
WHERE file.name != "_Index"
SORT choice(difficulty = "beginner", 1, choice(difficulty = "intermediate", 2, 3)) ASC, file.name ASC
```

## 📈 Patterns by Difficulty

### Beginner
```dataview
LIST
FROM "Concepts/Design-Patterns"
WHERE difficulty = "beginner" AND file.name != "_Index"
SORT file.name ASC
```

### Intermediate
```dataview
LIST
FROM "Concepts/Design-Patterns"
WHERE difficulty = "intermediate" AND file.name != "_Index"
SORT file.name ASC
```

### Advanced
```dataview
LIST
FROM "Concepts/Design-Patterns"
WHERE difficulty = "advanced" AND file.name != "_Index"
SORT file.name ASC
```

## 🏷️ By Category

### Creational Patterns
```dataview
LIST
FROM "Concepts/Design-Patterns"
WHERE contains(tags, "creational") AND file.name != "_Index"
SORT file.name ASC
```

### Structural Patterns
```dataview
LIST
FROM "Concepts/Design-Patterns"
WHERE contains(tags, "structural") AND file.name != "_Index"
SORT file.name ASC
```

### Behavioral Patterns
```dataview
LIST
FROM "Concepts/Design-Patterns"
WHERE contains(tags, "behavioral") AND file.name != "_Index"
SORT file.name ASC
```

## 🕐 Recently Updated

```dataview
TABLE dateformat(date-updated, "yyyy-MM-dd") AS "Last Updated"
FROM "Concepts/Design-Patterns"
WHERE file.name != "_Index"
SORT date-updated DESC
LIMIT 5
```
