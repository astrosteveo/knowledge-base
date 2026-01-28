# Writing Style Guide for Technical Documentation

This reference provides detailed guidance on tone, voice, and writing style for technical documentation in the Obsidian knowledge base.

## Tone & Voice Principles

### Clear and Direct
- Avoid unnecessary verbosity and filler words
- Get to the point quickly without sacrificing clarity
- Use simple sentence structures when possible
- Break complex ideas into digestible chunks

**Examples:**

✅ **Good:**
> "Optional prevents NullPointerException by forcing explicit null handling."

❌ **Too verbose:**
> "The Optional class is a feature that was introduced in order to help developers avoid the common problem of NullPointerException that tends to occur when values might potentially be null."

### Technically Precise
- Use correct technical terminology consistently
- Define terms on first use if not widely known
- Avoid ambiguous language
- Be specific about versions, conditions, and constraints

**Examples:**

✅ **Good:**
> "Introduced in Java 8, Optional is a container object that may or may not hold a non-null value."

❌ **Imprecise:**
> "Optional is a new thing in Java that helps with nulls."

### Educational Without Condescension
- Explain the "why" before or alongside the "how"
- Assume intelligence but not prior knowledge
- Use inclusive language ("we" not "you should know")
- Provide context for design decisions

**Examples:**

✅ **Good:**
> "The billion-dollar mistake in programming is the null reference. Optional makes the possibility of 'no value' explicit in the type system, forcing callers to handle this case."

❌ **Condescending:**
> "Obviously, you need to handle nulls. Use Optional because it's the right way to do things."

### Conversational Professionalism
- Write as if explaining to a colleague
- Use contractions naturally ("it's" not "it is")
- Be approachable but maintain authority
- Balance formality with readability

**Examples:**

✅ **Good:**
> "Think of Optional as a box that's either empty or contains exactly one item. It makes APIs clearer by signaling 'this method might not return a result.'"

❌ **Too formal:**
> "One must conceptualize Optional as a container entity with cardinality constraints."

❌ **Too casual:**
> "So like, Optional is basically just a wrapper thing lol"

## Use of Analogies

Analogies bridge abstract concepts to concrete understanding. Use them strategically for complex topics.

### When to Use Analogies
- Introducing abstract concepts (design patterns, algorithms)
- Explaining data structure behavior
- Clarifying performance characteristics
- Making functional programming concepts accessible

### Analogy Guidelines
1. **Choose familiar references** — Use everyday objects or experiences
2. **State the analogy explicitly** — "Think of X as Y" or "X is like Y"
3. **Follow with technical precision** — Don't let the analogy replace technical explanation
4. **Don't overextend** — Know when the analogy breaks down

**Examples:**

✅ **Good:**
> "Optional is like a box that's either empty or contains exactly one item. Unlike a regular box (a nullable reference), you can't peek inside without explicitly checking if it's empty first."

✅ **Good:**
> "A Stack is like a stack of plates — you can only add or remove from the top (Last-In-First-Out)."

✅ **Good:**
> "Binary search is like finding a name in a phone book by repeatedly opening to the middle and eliminating half the remaining pages."

❌ **Too vague:**
> "Optional is kind of like a thing that wraps stuff."

❌ **Overextended:**
> "Optional is like a Schrödinger's cat where the value is both present and absent until you observe it with get()..." [This misleads more than clarifies]

## Code Example Philosophy

### 1. Progression: Simple to Complex

Start with the simplest working example, then layer in complexity.

**Structure:**
- **Basic Usage:** "Hello World" level — introduces core concept
- **Intermediate Example:** Real-world pattern with common use case
- **Advanced Usage:** Edge cases, combinations, performance considerations

**Example progression for Optional:**

```java
// Basic: Creating and extracting values
Optional<String> name = Optional.of("Alice");
String result = name.orElse("Unknown");  // "Alice"

// Intermediate: Chaining operations
Optional<String> email = findUserById(123)
    .map(User::getEmail)
    .filter(e -> e.contains("@"))
    .map(String::toLowerCase);

// Advanced: Complex chaining with fallbacks
String displayName = findUserById(userId)
    .flatMap(User::getPreferredName)
    .or(() -> findUserById(userId).map(User::getFullName))
    .or(() -> findUserById(userId).map(User::getUsername))
    .orElse("Guest");
```

### 2. Practical: Real-World Over Toy Examples

Prefer examples that solve actual problems over contrived scenarios.

✅ **Good (practical):**
```java
// Finding a configuration value with fallback chain
String apiUrl = config.get("api.url")
    .or(() -> config.get("legacy.api.endpoint"))
    .orElseGet(() -> System.getenv("API_URL"))
    .orElse("https://api.default.com");
```

❌ **Less useful (toy example):**
```java
Optional<Integer> num = Optional.of(42);
int doubled = num.map(n -> n * 2).orElse(0);
```

### 3. Runnable: Executable Code Only

Every code example must be valid, syntactically correct, and executable.

**Requirements:**
- Include necessary imports if not obvious
- Use realistic types and method signatures
- Show complete context for standalone snippets
- Avoid pseudocode entirely

✅ **Good (runnable):**
```java
import java.util.Optional;

public class UserService {
    public Optional<User> findById(Long id) {
        // Query database
        User user = repository.findById(id);
        return Optional.ofNullable(user);
    }
}
```

❌ **Bad (pseudocode):**
```
optional = create_optional(value)
if optional.has_value():
    process(optional.value)
```

### 4. Commented: Explain Non-Obvious Logic

Add inline comments for complex operations, but don't over-comment obvious code.

✅ **Good commenting:**
```java
// Combine multiple Optional sources with priority fallback
return primaryConfig.get(key)
    .or(() -> secondaryConfig.get(key))  // Try fallback config
    .or(() -> Optional.ofNullable(System.getenv(key.toUpperCase())))  // Check environment
    .filter(v -> !v.isEmpty())  // Reject empty strings
    .orElseThrow(() -> new ConfigurationException("Missing required key: " + key));
```

❌ **Over-commented:**
```java
Optional<String> opt = Optional.of("hello");  // Create an Optional
String result = opt.get();  // Get the value
System.out.println(result);  // Print the result
```

### 5. Multiple Variations: Language-Specific Approaches

When documenting cross-language concepts, show idiomatic implementations in each language.

**Example: Optional pattern across languages**

```java
// Java: Optional<T>
Optional<User> user = findUser(id);
String name = user.map(User::getName).orElse("Guest");
```

```python
# Python: None with optional chaining
user = find_user(id)  # Returns User or None
name = user.name if user else "Guest"
```

```javascript
// JavaScript: Null coalescing and optional chaining
const user = findUser(id);
const name = user?.name ?? "Guest";
```

## Content Completeness Standards

### Minimum Requirements

Every technical document must include:

1. **3+ code examples** with progression (basic → intermediate → advanced)
2. **What to do AND what NOT to do** (✅ Good vs ❌ Bad examples)
3. **Performance considerations** (for algorithms/data structures)
4. **Thread safety considerations** (for concurrent Java features)
5. **Common pitfalls** explicitly documented

### Performance Considerations (Algorithms/Data Structures)

Include time and space complexity analysis:

```markdown
## Performance

### Time Complexity
- **Insert:** O(log n) — Binary tree traversal
- **Search:** O(log n) — Binary tree traversal
- **Delete:** O(log n) — Find + rebalance

### Space Complexity
- O(n) — Tree nodes storage
- O(log n) — Recursion stack depth (worst case: O(n) for skewed tree)

### When to Use
- Need sorted traversal: Tree maintains sort order
- Frequent insertions/deletions: Better than array
- Range queries: Efficient in-order traversal

### Trade-offs
- More memory overhead than arrays
- Slower than hash tables for point lookups
- Requires rebalancing for optimal performance
```

### Thread Safety Considerations (Java)

For concurrent features, document safety guarantees:

```markdown
## Thread Safety

**Not thread-safe by default:**
- ArrayList operations are not synchronized
- Concurrent modification throws ConcurrentModificationException

**Safe alternatives:**
- `Collections.synchronizedList()` — Wraps with synchronized methods
- `CopyOnWriteArrayList` — Thread-safe, best for read-heavy workloads
- `Vector` — Legacy synchronized list (avoid in new code)

**Safe usage patterns:**
```java
// External synchronization required
List<String> list = new ArrayList<>();
synchronized(list) {
    if (list.isEmpty()) {
        list.add("first");
    }
}
```
```

### Common Pitfalls Documentation

Explicitly call out mistakes and misconceptions:

```markdown
## Common Pitfalls

> [!warning] Don't Use get() Without Checking
> Calling `get()` on an empty Optional throws NoSuchElementException. Always use orElse, orElseGet, or check with isPresent() first.

❌ **Bad:**
```java
Optional<String> opt = findValue();
String value = opt.get();  // Throws if empty!
```

✅ **Good:**
```java
String value = findValue().orElse("default");
// Or: if (opt.isPresent()) { ... }
```
```

## Writing for Different Audiences

### Beginner Documents
- Define all technical terms
- Use more analogies and visualizations
- Provide step-by-step explanations
- Link to prerequisite concepts
- Show complete working examples (no shortcuts)

### Intermediate Documents
- Assume familiarity with basics
- Focus on practical patterns and best practices
- Explain "why" certain approaches are preferred
- Show trade-offs between alternatives
- Include performance considerations

### Advanced Documents
- Assume strong foundational knowledge
- Focus on edge cases and optimizations
- Deep-dive into implementation details
- Reference academic papers and research
- Discuss design philosophy and trade-offs

## Formatting Standards

### Headings Hierarchy
- `# Title` — Document title (matches frontmatter title)
- `## Section` — Major sections (Theory, Examples, Patterns)
- `### Subsection` — Subsections (Basic Usage, Intermediate Example)
- `#### Detail` — Rarely used, for fine-grained breakdown

### Lists and Bullets
- Use **bulleted lists** for unordered concepts
- Use **numbered lists** for sequential steps or ranked items
- Use **definition lists** for term-explanation pairs (when Markdown supports it)

### Emphasis
- **Bold** for emphasis and key terms
- *Italics* for technical terms on first use or book titles
- `Code` for inline code, methods, classes, variables
- Never use ALL CAPS for emphasis

### Code Blocks
Always specify language for syntax highlighting:

````markdown
```java
public class Example { }
```

```python
def example():
    pass
```

```bash
echo "Hello"
```
````

### Callouts
Use Obsidian callout syntax consistently:

```markdown
> [!summary]
> Document overview (3-5 sentences)

> [!tip] Best Practice
> Recommendation with explanation

> [!warning] Common Mistake
> What to avoid and why

> [!info] Additional Context
> Supplementary information
```

## Cross-Referencing Standards

### Internal Links
- Use wiki-link syntax: `[[Document-Name]]`
- Link to exact filenames without `.md` extension
- Link to category indexes: `[[Languages/Java/_Index]]`
- Create bidirectional links in "Related Topics" section

### Related Topics Section
Every document should end with related topics:

```markdown
## Related Topics

- [[Streams]] — Optional integrates with Java Streams
- [[Null-Safety]] — Broader null handling strategies
- [[Functional-Programming]] — map, flatMap, and functional composition
- [[Languages/Java/_Index]] — Java features index
```

### References Section
Cite authoritative sources:

```markdown
## References

- [Java Optional Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)
- [Optional API Design](https://www.oracle.com/technical-resources/articles/java/java8-optional.html)
- Bloch, Joshua. *Effective Java*, 3rd ed. Item 55: Return optionals judiciously
- [Monad Pattern](https://en.wikipedia.org/wiki/Monad_(functional_programming))
```

## Language-Specific Style Notes

### Java
- Mention Java version when features were introduced
- Show both imperative and functional approaches
- Document serialization behavior for classes
- Include thread safety analysis
- Reference *Effective Java* where applicable

### Python
- Follow PEP 8 naming conventions in examples
- Show Pythonic idioms (list comprehensions, context managers)
- Document Python version requirements
- Use type hints in examples (Python 3.5+)

### Shell/Bash
- Include shebang lines: `#!/bin/bash`
- Quote variables: `"$var"` not `$var`
- Use `set -e` for error handling in scripts
- Explain flag meanings inline

### Algorithms/Data Structures
- Always include Big-O analysis
- Provide visual diagrams (Mermaid)
- Show multiple implementations when applicable
- Document space-time trade-offs

### Prompting Techniques
- Include real conversation examples
- Show both input prompts and expected outputs
- Cite research papers (arXiv links)
- Document when to use each technique
- Show before/after comparisons

## Revision and Maintenance

### When Updating Documents
1. Read entire document first
2. Update `date-updated` in frontmatter
3. Maintain existing structure and voice
4. Add, don't replace (preserve working examples)
5. Update status if maturity changed
6. Verify all links still work

### Status Progression
- **seed** 🌱 — Basic outline, < 1000 words, minimal examples
- **growing** 🌿 — Substantial content, 1000-3000 words, 2+ examples
- **evergreen** 🌳 — Complete, 3000+ words, 3+ examples, comprehensive

### Quality Indicators for Evergreen Status
- All 7 sections present and complete
- Minimum 3 code examples with progression
- Performance analysis (if applicable)
- Thread safety documentation (if applicable)
- Common pitfalls explicitly documented
- Related topics cross-referenced
- Authoritative sources cited
- Mermaid diagrams for complex concepts

## Anti-Patterns to Avoid

### ❌ Don't: Assume Prior Knowledge Without Links
Bad: "Use the Visitor pattern here."
Good: "Use the [[Visitor-Pattern]] to separate algorithms from object structure."

### ❌ Don't: Use Vague Language
Bad: "This is usually faster."
Good: "This runs in O(log n) compared to O(n) for linear search."

### ❌ Don't: Skip Error Handling in Examples
Bad:
```java
String result = optional.get();  // Throws if empty
```

Good:
```java
String result = optional.orElse("default");
// Or: String result = optional.orElseThrow(() -> new CustomException());
```

### ❌ Don't: Use Inconsistent Terminology
Bad: Switching between "method," "function," and "procedure" randomly
Good: Use "method" consistently for Java, "function" for Python

### ❌ Don't: Write Without Testing Code
Bad: Including code examples without running them
Good: Test all code examples for syntax and logic errors

### ❌ Don't: Overuse Passive Voice
Bad: "The value is returned by the method."
Good: "The method returns the value."

### ❌ Don't: Include Outdated Information
Bad: "Java 8 is the latest version."
Good: "Introduced in Java 8, Optional is available in all modern Java versions."
