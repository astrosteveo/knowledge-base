---
title: Few-Shot with Negative Examples
category: Concepts/Prompting-Techniques
tags:
  - prompting
  - ai
  - few-shot
  - examples
  - anthropic
difficulty: beginner
status: evergreen
date-created: 2025-12-09
date-updated: 2025-12-09
sources:
  - https://docs.anthropic.com/claude/docs/prompt-engineering
---

# Few-Shot with Negative Examples

> [!summary]
> Show the model what NOT to do alongside positive examples. Anthropic's research shows that negative examples with explanations eliminate 80% of generic AI responses by defining clear boundaries around acceptable output.

## Theory

### What Is Few-Shot with Negative Examples?

Standard few-shot prompting shows good examples. This technique adds bad examples with explicit explanations of why they fail. The model learns both the target pattern and the anti-patterns to avoid.

### How It Works

The contrast between good and bad examples creates sharper boundaries. Explaining *why* bad examples fail teaches the underlying principle, not just the surface pattern.

```
Good examples define: "Output should look like this"
Bad examples define: "Output must never look like this"
Explanations teach: "Here's why the distinction matters"
```

## Practical Examples

### Template

```
I need you to [task]. Here are examples:

GOOD Example 1: [example]
GOOD Example 2: [example]

BAD Example 1: [example]
Why it's bad: [reason]
BAD Example 2: [example]
Why it's bad: [reason]

Now complete: [your actual task]
```

### Basic Usage

```
I need you to write cold email subject lines. Here are examples:

GOOD: "Quick question about your Q4 engineering roadmap"
GOOD: "Saw your post on distributed systems—thoughts on this?"

BAD: "URGENT: Limited Time Offer Inside!!!"
Why it's bad: Spam trigger words, manufactured urgency
BAD: "You won't believe what we built..."
Why it's bad: Clickbait pattern, no context

Now write 5 subject lines for: SaaS tool that reduces cloud costs by 40%
```

### Advanced Usage

```
I need you to write error messages for a developer CLI tool. Here are examples:

GOOD: "Config file not found at ~/.myapp/config.yaml. Run 'myapp init' to create one."
GOOD: "Authentication failed: token expired. Run 'myapp auth refresh' to get a new token."

BAD: "Error: Something went wrong"
Why it's bad: No actionable information, no error code, no recovery path
BAD: "FATAL ERROR!!! Cannot proceed. Contact support immediately."
Why it's bad: Alarmist tone, no details, pushes responsibility to support
BAD: "Error code 0x4A2F: ENOENT in syscall at addr 0x7fff5fbff8c0"
Why it's bad: Raw technical details without context or next steps

Now write error messages for:
1. Database connection timeout
2. Invalid API key format
3. Rate limit exceeded
```

## Common Patterns

> [!tip] Explain the Principle, Not Just the Symptom
> "Why it's bad: too long" is weak. "Why it's bad: exceeds 50 chars, gets truncated in mobile notifications" teaches the underlying constraint.

> [!tip] Match Bad Examples to Common Failures
> If you've seen the AI produce certain bad outputs before, use those as negative examples. Target the specific failure modes you want to prevent.

> [!warning] Balance Positive and Negative
> Too many negative examples can confuse the model about what you actually want. Aim for 2-3 of each.

## Edge Cases & Gotchas

- **Overly specific negatives** — If bad examples are too narrow, the model might just avoid those exact patterns while finding new failure modes.
- **Contradictory examples** — Ensure good and bad examples don't overlap. A "concise" good example and a "too short" bad example create confusion.
- **Missing the why** — Negative examples without explanations teach pattern-matching, not understanding. Always explain the reason.
- **Cultural/context blindness** — What's "bad" depends on context. A casual tone is bad for legal documents but good for marketing copy.

## Related Topics

- [[Role-Based-Constraint-Prompting]] - Complements few-shot with persona constraints
- [[Constraint-First-Prompting]] - Explicit constraints instead of examples
- [[Meta-Prompting]] - Generate optimal examples automatically

## References

- [Anthropic Prompt Engineering](https://docs.anthropic.com/claude/docs/prompt-engineering)
- [Few-Shot Learning Research](https://arxiv.org/abs/2005.14165)
