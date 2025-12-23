---
title: Confidence-Weighted Prompting
category: Concepts/Prompting-Techniques
tags:
  - prompting
  - ai
  - confidence
  - uncertainty
  - decision-making
  - deepmind
difficulty: intermediate
status: evergreen
date-created: 2025-12-09
date-updated: 2025-12-09
sources:
  - https://deepmind.google/research/
---

# Confidence-Weighted Prompting

> [!summary]
> Require the model to rate its confidence, state assumptions, identify what would change its answer, and provide alternatives when uncertain. This prevents decisions based on false certainty.

## Theory

### What Is Confidence-Weighted Prompting?

Google DeepMind developed this technique for high-stakes decisions. The model must:

1. Provide a primary answer
2. Rate confidence (0-100%)
3. List key assumptions
4. State what would change the answer
5. Offer an alternative if confidence is below threshold

This surfaces uncertainty that models typically hide behind confident-sounding prose.

### How It Works

Models default to confident delivery regardless of actual certainty. Forcing explicit confidence ratings and alternatives reveals the true reliability of the response.

Low confidence + clear alternatives = informed decision-making
High confidence + weak assumptions = proceed with caution

## Practical Examples

### Template

```
Answer this question: [question]

For your answer, provide:
1. Your primary answer
2. Confidence level (0-100%)
3. Key assumptions you're making
4. What would change your answer
5. Alternative answer if you're <80% confident
```

### Basic Usage

```
Answer this question: Will Rust replace C++ in systems programming by 2030?

For your answer, provide:
1. Your primary answer
2. Confidence level (0-100%)
3. Key assumptions you're making
4. What would change your answer
5. Alternative answer if you're <80% confident
```

### Advanced Usage

```
Answer this question: Should we rewrite our Python ML pipeline in Go for the 3x performance improvement?

For your answer, provide:
1. Your primary recommendation with specific reasoning
2. Confidence level (0-100%) with justification for the rating
3. Key assumptions about:
   - Current bottleneck locations
   - Team Go expertise
   - Maintenance burden
   - Timeline constraints
4. Factors that would change your recommendation:
   - What data would make you more confident?
   - What conditions would flip your answer?
5. Alternative recommendation if confidence <80%
6. Suggested experiments to increase confidence
```

## Common Patterns

> [!tip] Set Explicit Thresholds
> "If confidence <80%, provide alternative" creates clear decision points. Adjust thresholds based on stakes: 95% for medical/legal, 70% for exploratory decisions.

> [!tip] Ask What Would Change the Answer
> This question often reveals the most important factors. If the answer would change with "more data on X," that tells you what to investigate.

> [!warning] Confidence Isn't Calibrated
> Model confidence percentages aren't statistically calibrated. Use them as relative indicators, not absolute probabilities. 70% confidence just means "notably uncertain."

## Edge Cases & Gotchas

- **Anchoring on round numbers** — Models often give 70%, 80%, 90%. Ask for specific percentages with justification.
- **Overconfidence on training data** — High confidence on topics well-represented in training data; lower confidence is warranted for recent or niche topics.
- **Alternative quality varies** — Sometimes the "alternative" is weak or just restates the primary answer negatively. Require genuinely different approaches.
- **Assumption blind spots** — The model might miss critical assumptions. Consider prompting for specific categories: technical, business, timeline, resource assumptions.

## Related Topics

- [[Chain-of-Verification]] - Verify claims before rating confidence
- [[Multi-Perspective-Prompting]] - Multiple viewpoints reveal uncertainty
- [[Structured-Thinking-Protocol]] - Structured analysis before confidence rating

## References

- [DeepMind Research](https://deepmind.google/research/)
- [Calibration in LLMs](https://arxiv.org/abs/2207.05221)
