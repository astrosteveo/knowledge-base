---
title: Role-Based Constraint Prompting
category: Concepts/Prompting-Techniques
tags:
  - prompting
  - ai
  - constraints
  - roles
difficulty: beginner
status: evergreen
date-created: 2025-12-09
date-updated: 2025-12-09
sources:
  - https://www.anthropic.com/research
---

# Role-Based Constraint Prompting

> [!summary]
> Assign the AI an expert role with specific constraints instead of making vague requests. This technique produces outputs 10x more specific than generic prompts by defining expertise, task scope, limitations, and output format upfront.

## Theory

### What Is Role-Based Constraint Prompting?

This technique structures prompts with four elements:

1. **Role** — A specific expert identity with years of experience
2. **Task** — A concrete, measurable objective
3. **Constraints** — 3-5 specific limitations or requirements
4. **Output format** — The exact structure needed

Generic prompts yield generic answers. Constrained roles yield focused expertise.

### How It Works

The AI adopts the specified persona and filters its responses through that lens. Constraints eliminate irrelevant options. The output format ensures usable results.

## Practical Examples

### Template

```
You are a [specific role] with [X years] experience in [domain].
Your task: [specific task]
Constraints:
- [constraint 1]
- [constraint 2]
- [constraint 3]
Output format: [exact format needed]
```

### Basic Usage

```
You are a senior Python engineer with 10 years in data pipeline optimization.
Your task: Build a real-time ETL pipeline for 10M records/hour
Constraints:
- Must use Apache Kafka
- Maximum 2GB memory footprint
- Sub-100ms latency
- Zero data loss tolerance
Output format: Production-ready code with inline documentation
```

### Advanced Usage

```
You are a principal security architect with 15 years in financial systems.
Your task: Design authentication for a trading platform
Constraints:
- Must support hardware tokens and biometrics
- Session timeout under 5 minutes for inactive users
- Audit logging for all auth events
- Zero trust architecture principles
- SOC 2 Type II compliance required
Output format: Architecture diagram description, implementation steps, and security considerations
```

## Common Patterns

> [!tip] Be Specific About Experience Level
> "Senior engineer with 10 years" produces different output than "junior developer." Match the role to your needs.

> [!tip] Constraints Drive Quality
> More specific constraints yield more specific outputs. "Must handle errors" is weak. "Must retry 3 times with exponential backoff, then fail gracefully with structured logging" is strong.

> [!warning] Avoid Role Conflicts
> Don't assign conflicting expertise. A "frontend developer with backend optimization experience" muddies the focus. Pick one primary role.

## Edge Cases & Gotchas

- **Too many constraints** — More than 5-6 constraints can confuse the model. Prioritize the essential ones.
- **Vague roles** — "Expert programmer" is too broad. Specify the domain: "Rust systems programmer specializing in memory-safe concurrency."
- **Missing output format** — Without a specified format, you'll get whatever structure the AI chooses.
- **Unrealistic combinations** — Asking for "zero latency" and "comprehensive logging" creates tension. Acknowledge tradeoffs.

## Related Topics

- [[Constraint-First-Prompting]] - Alternative approach that leads with constraints
- [[Structured-Thinking-Protocol]] - Adds reasoning structure to role-based prompts
- [[Meta-Prompting]] - Let the AI design the optimal role and constraints

## References

- [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/claude/docs/prompt-engineering)
