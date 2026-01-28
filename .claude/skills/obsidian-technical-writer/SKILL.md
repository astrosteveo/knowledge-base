---
name: obsidian-technical-writer
description: This skill should be used when the user asks to "write documentation", "create a technical document", "document this code", "add to knowledge base", "write about [topic]", "create index file", "update documentation", or wants to create professionally written technical documentation following software development standards and Obsidian vault conventions defined in CLAUDE.md.
---

# Obsidian Technical Writer

## Purpose

Write professionally formatted technical documentation for an Obsidian knowledge base vault following established conventions, writing standards, and structural templates. Produce documentation that matches the quality and style of existing vault content with proper frontmatter, visual elements, code examples, and cross-references.

## When to Use

Use this skill when:
- Creating new technical documents (language features, algorithms, design patterns, CLI tools)
- Writing or updating category index files
- Documenting code, APIs, or technical concepts
- Adding content to an existing knowledge base
- Converting informal notes to formal documentation
- Updating existing documents to match vault standards

## Core Workflow

### Step 1: Understand the Context

Before writing, gather essential information:

1. **Read CLAUDE.md** from the vault root to understand:
   - Directory structure and category organization
   - File naming conventions (Title-Case-With-Hyphens.md)
   - Frontmatter requirements
   - 7-section document structure
   - Writing style guidelines
   - Visual element standards

2. **Identify the document type** to determine the appropriate template:
   - Standard technical document (language features, design patterns)
   - Index file (_Index.md for categories)
   - Algorithm/data structure (with complexity analysis)
   - Prompting technique (with LLM examples)
   - Shell/CLI tool (with command reference)
   - Framework feature (framework-specific patterns)

3. **Read similar documents** in the target category to match:
   - Writing style and tone
   - Code example patterns
   - Visual element usage
   - Cross-referencing conventions

### Step 2: Choose the Correct Category

Place documents in the appropriate directory:

- **Languages/[Language]/** — Language-specific syntax, features, APIs (e.g., Java Optional, Python decorators)
- **Frameworks/[Framework]/** — Framework-specific patterns and best practices (e.g., React hooks, Spring annotations)
- **Concepts/Algorithms/** — Language-agnostic algorithms (e.g., binary search, merge sort)
- **Concepts/Data-Structures/** — Data structures (e.g., trees, graphs, hash tables)
- **Concepts/Design-Patterns/** — Design patterns (e.g., Singleton, Factory, Observer)
- **Concepts/Prompting-Techniques/** — LLM interaction strategies (e.g., chain-of-thought, few-shot prompting)
- **Languages/Shell/** — Shell commands and CLI tools (e.g., grep, awk, sed)

### Step 3: Create Complete Frontmatter

Every document starts with YAML frontmatter:

```yaml
---
title: Topic Name
category: Category/Subcategory
tags:
  - primary-tag
  - secondary-tag
  - feature-type-tag
difficulty: beginner  # beginner | intermediate | advanced
status: seed  # seed | growing | evergreen
date-created: YYYY-MM-DD
date-updated: YYYY-MM-DD
sources:
  - https://official-documentation-url
  - https://additional-reference
---
```

**Difficulty levels:**
- `beginner` — Foundational concepts, suitable for newcomers
- `intermediate` — Requires basic knowledge, real-world patterns
- `advanced` — Complex scenarios, edge cases, performance considerations

**Status levels:**
- `seed` 🌱 — Basic outline, minimal content (< 1000 words, < 2 examples)
- `growing` 🌿 — Substantial content (1000-3000 words, 2+ examples)
- `evergreen` 🌳 — Complete, comprehensive (3000+ words, 3+ examples, all sections)

### Step 4: Write Using the 7-Section Structure

Follow this structure for standard technical documents:

1. **Summary Callout** — 3-5 sentence overview using `> [!summary]`
2. **Theory Section** — "What Is [Topic]?" and "How It Works" with Mermaid diagrams
3. **Quick Reference** (Optional) — Tables for APIs, commands, or methods
4. **Practical Examples** — Minimum 3 examples (basic → intermediate → advanced)
5. **Common Patterns** — Best practices with ✅ good and ❌ bad examples
6. **Edge Cases & Gotchas** — Bullet list of pitfalls and considerations
7. **Related Topics & References** — Cross-links and authoritative sources

Consult `references/document-templates.md` for complete templates for each document type.

### Step 5: Write in the Established Style

Follow these writing principles:

**Tone:**
- Clear and direct (avoid verbosity)
- Technically precise (correct terminology)
- Educational without condescension (explain "why" not just "how")
- Conversational professionalism (approachable but authoritative)
- Use analogies for complex concepts

**Code Examples:**
- **Progression:** Start simple, build to complex (basic → intermediate → advanced)
- **Practical:** Real-world scenarios over toy examples
- **Runnable:** Every snippet must be executable (never pseudocode)
- **Commented:** Explain non-obvious logic
- **Multiple variations:** Show language-specific approaches when applicable

**Content Completeness:**
- Minimum 3 code examples with clear progression
- Both what to do (✅ Good) and what NOT to do (❌ Bad)
- Performance considerations (for algorithms/data structures)
- Thread safety considerations (for Java concurrent features)
- Common pitfalls explicitly documented

See `references/writing-style-guide.md` for detailed style guidelines with examples.

### Step 6: Add Visual Elements

Enhance understanding with appropriate diagrams and tables:

**Mermaid Diagrams:**
- `flowchart` — Algorithm flows, process diagrams
- `classDiagram` — Class relationships, API structure
- `sequenceDiagram` — Method calls, interaction flows
- `stateDiagram` — State machines, lifecycles
- `erDiagram` — Database schemas

**LaTeX Math Notation:**
- Inline: `$O(n \log n)$` for complexity
- Block: `$$formula$$` for equations

**Tables:**
- API/method reference tables
- Time/space complexity comparisons
- Feature comparison tables
- CLI flag/option reference

**Callouts:**
- `> [!summary]` — Document overview
- `> [!tip]` — Best practices
- `> [!warning]` — Common mistakes
- `> [!info]` — Additional context

Consult `references/visual-elements-guide.md` for comprehensive diagram syntax and examples.

### Step 7: Cross-Reference Related Topics

Create bidirectional links:

1. **Add "Related Topics" section** at document end with wiki-links:
   ```markdown
   ## Related Topics

   - [[Related-Topic-1]] — Brief relationship description
   - [[Related-Topic-2]] — Brief relationship description
   - [[Category/_Index]] — Link back to category index
   ```

2. **Update the category index** to include the new document:
   - Add entry to appropriate difficulty table
   - Include status emoji (🌱🌿🌳)
   - Provide short description

3. **Add to learning path** in index if appropriate for recommended study order

### Step 8: Cite Authoritative Sources

Always include a "References" section:

```markdown
## References

- [Official Documentation](https://official-url)
- [Tutorial or Guide](https://tutorial-url)
- [Academic Paper](https://arxiv.org/...) (if applicable)
- Book: *Author. Title.* Edition. Publisher, Year. ISBN.
```

## Index File Workflow

Index files (`_Index.md`) have a special structure:

1. **Summary callout** — Category overview and learning value
2. **Overview** — 2-3 paragraphs of context
3. **Learning Path** — Beginner/Intermediate/Advanced sections with links
4. **Topics** — Tables grouped by subcategory with difficulty/status/description
5. **Recently Updated** — Dataview query showing latest changes
6. **Quick Reference** — Category-specific reference material
7. **Related Categories** — Cross-links to other indexes

Consult `references/document-templates.md` for the complete index file template.

## Document Type-Specific Guidelines

### Language Features (Java, Python)
- Include version information (e.g., "Introduced in Java 8")
- Show both imperative and functional approaches
- Document thread safety for concurrent features
- Cover serialization implications if relevant

### Algorithms/Data Structures
- Always include Big-O complexity analysis (time and space)
- Provide Mermaid diagrams visualizing structure/flow
- Show multiple implementation approaches
- Document practical use cases and trade-offs

### Prompting Techniques
- Include real LLM conversation examples
- Show both input prompts and expected outputs
- Document when to use each technique
- Cite research papers (arXiv links)

### Shell/CLI Tools
- Include common flags and options table
- Show command patterns and pipelines
- Document cross-platform differences
- Explain safety considerations

## Quality Standards Checklist

Before considering a document complete:

- [ ] Frontmatter complete and accurate
- [ ] Summary callout present (3-5 sentences)
- [ ] Theory section explains "what" and "why"
- [ ] Minimum 3 code examples (basic, intermediate, advanced)
- [ ] All code is runnable with proper syntax highlighting
- [ ] Common patterns section with tips and warnings
- [ ] Edge cases and gotchas documented
- [ ] Related topics linked with wiki-links
- [ ] References cite authoritative sources
- [ ] Mermaid diagrams for complex concepts (if applicable)
- [ ] Cross-referenced from category index
- [ ] Writing style matches vault conventions

## Updating Existing Documents

When updating existing documents:

1. **Read the entire document first** to understand current content
2. **Update `date-updated` in frontmatter**
3. **Maintain existing structure** and formatting conventions
4. **Match the established tone** and writing style
5. **Preserve all working code examples** (only fix if broken)
6. **Add, don't replace** — Expand content rather than removing
7. **Update status** if content maturity has changed (seed → growing → evergreen)
8. **Verify all internal links** still resolve correctly

## Additional Resources

### Reference Files

For detailed guidance, consult:

- **`references/writing-style-guide.md`** — Comprehensive tone, voice, and style guidelines with examples
- **`references/document-templates.md`** — Complete templates for all document types (technical, index, algorithm, prompting, CLI, framework)
- **`references/visual-elements-guide.md`** — Mermaid diagram syntax, LaTeX math notation, table formatting, and callout usage

### Example Files

Working examples demonstrating all conventions:

- **`examples/example-technical-document.md`** — Complete Java Streams document showing all 7 sections, frontmatter, visual elements, and writing style
- **`examples/example-index-file.md`** — Complete Data Structures index showing organization, Dataview queries, and cross-referencing

## Common Patterns

> [!tip] Start with Examples, Then Formalize
> When documenting unfamiliar topics, first write practical code examples to understand the concept, then structure into the 7-section template.

> [!tip] Use Similar Documents as Templates
> Read 2-3 documents in the target category to understand specific conventions before writing new content.

> [!tip] Progressive Disclosure in Code Examples
> Build complexity gradually: basic example introduces core concept, intermediate shows realistic usage, advanced demonstrates edge cases or optimizations.

> [!warning] Don't Skip the Summary Callout
> The summary is the most important section — it determines whether readers will engage with the full document. Spend time crafting a clear, compelling overview.

> [!warning] Verify Code is Runnable
> Never include pseudocode or untested examples. All code must be syntactically correct and executable.

## Implementation Tips

1. **Read CLAUDE.md first** — Critical for understanding vault conventions
2. **Use the reference files** — Don't try to remember all details
3. **Study existing documents** — Match the established patterns
4. **Start with frontmatter** — Ensures proper categorization from the beginning
5. **Write examples early** — Often reveals gaps in theory section
6. **Add visuals as you write** — Don't save diagrams for later
7. **Cross-reference as you go** — Easier than retrofitting links
8. **Update the index immediately** — Ensures discoverability

Create documentation that is comprehensive, clear, and consistent with vault conventions. The goal is evergreen content that serves as both learning material and quick reference for technical concepts.