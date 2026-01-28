---
title: Prompting Techniques Index
date-updated: 2025-12-10
---

# Prompting Techniques

> [!summary]
> Advanced prompting techniques transform vague AI requests into precise, high-quality outputs. These methods—drawn from research at Google, OpenAI, and Anthropic—structure prompts to reduce hallucinations, improve reasoning, and produce production-ready results.

## Recently Updated

```dataview
TABLE difficulty, status, file.mtime as "Modified"
FROM "Concepts/Prompting-Techniques"
WHERE file.name != "_Index"
SORT file.mtime DESC
LIMIT 5
```

## By Status

### 🌱 Seeds (need expansion)
```dataview
LIST FROM "Concepts/Prompting-Techniques" WHERE status = "seed" AND file.name != "_Index"
```

### 🌿 Growing
```dataview
LIST FROM "Concepts/Prompting-Techniques" WHERE status = "growing" AND file.name != "_Index"
```

### 🌳 Evergreen (comprehensive)
```dataview
LIST FROM "Concepts/Prompting-Techniques" WHERE status = "evergreen" AND file.name != "_Index"
```

## All Topics

```dataview
TABLE difficulty, status
FROM "Concepts/Prompting-Techniques"
WHERE file.name != "_Index"
SORT file.name ASC
```

## Core Techniques

### Foundational
- [[Role-Based-Constraint-Prompting]] — Assign expert roles with specific constraints
- [[Constraint-First-Prompting]] — Define hard limits before stating the task

### Quality & Accuracy
- [[Chain-of-Verification]] — Self-verify to eliminate hallucinations
- [[Few-Shot-with-Negative-Examples]] — Show what NOT to do alongside good examples
- [[Confidence-Weighted-Prompting]] — Rate confidence and provide alternatives

### Structured Reasoning
- [[Structured-Thinking-Protocol]] — Force layered analysis before answering
- [[Multi-Perspective-Prompting]] — Analyze from multiple viewpoints
- [[Context-Injection-with-Boundaries]] — Inject context with strict usage rules

### Iterative Methods
- [[Iterative-Refinement-Loop]] — Refine outputs through multiple passes
- [[Meta-Prompting]] — Let the AI write its own optimal prompt

## When to Use What

| Goal | Technique |
|------|-----------|
| More specific outputs | [[Role-Based-Constraint-Prompting]] |
| Reduce hallucinations | [[Chain-of-Verification]], [[Context-Injection-with-Boundaries]] |
| Eliminate generic responses | [[Few-Shot-with-Negative-Examples]] |
| Complex reasoning | [[Structured-Thinking-Protocol]] |
| High-stakes decisions | [[Confidence-Weighted-Prompting]] |
| Technical constraints | [[Constraint-First-Prompting]] |
| Strategic analysis | [[Multi-Perspective-Prompting]] |
| Quality improvement | [[Iterative-Refinement-Loop]] |
| Optimal prompts | [[Meta-Prompting]] |

## Claude Code Best Practices

A comprehensive guide series for working effectively with Claude Code, covering everything from fundamentals to advanced techniques.

### Start Here
- [[Claude-Code-Best-Practices]] — Hub document with overview and navigation

### Essential Skills (All Users)
- [[Prompt-Engineering-for-Claude-Code]] — Write clear, effective requests
- [[Context-Management-Strategies]] — Provide the right information at the right time
- [[Safety-Practices-for-AI-Coding]] — Prevent destructive operations and establish safe workflows

### Problem Solving
- [[Debugging-Claude-Code-Outputs]] — Handle mistakes and course-correct effectively

### Power User Techniques
- [[Advanced-Claude-Code-Techniques]] — Parallel execution, agents, optimization, and expert workflows

**Recommended Path:** Start with the hub document, then read Essential Skills in order. Reference Debugging as needed. Explore Advanced Techniques once you're comfortable with the basics.

## See Also

- [[../Algorithms/_Index|Algorithms]] — Systematic problem-solving approaches
- [[../Design-Patterns/_Index|Design Patterns]] — Reusable solution templates
