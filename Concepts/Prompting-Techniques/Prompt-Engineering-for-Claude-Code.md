---
title: Prompt Engineering for Claude Code
category: Concepts/Prompting-Techniques
tags:
  - claude-code
  - prompt-engineering
  - clarity
  - specification
difficulty: beginner
status: evergreen
date-created: 2026-01-13
date-updated: 2026-01-13
sources:
  - https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
  - https://github.com/anthropics/claude-code
---

> [!summary]
> Master the art of writing clear, effective requests for Claude Code. This guide teaches you how to structure prompts that get consistently excellent results by providing proper context, setting clear boundaries, and specifying requirements without over-constraining solutions. Learn to close the gap between what you mean and what you say.

## What Is Prompt Engineering for Claude Code?

Prompt engineering is the practice of crafting requests that communicate your intent clearly and provide Claude with the information needed to succeed. Unlike general-purpose chatbots, Claude Code operates in a coding environment where:

- **Ambiguity is expensive** — A misunderstood request leads to wasted code, files, and time
- **Context is critical** — Claude needs to understand your codebase, stack, and constraints
- **Precision matters** — "Add authentication" could mean OAuth, JWT, sessions, API keys, or magic links
- **Actions are permanent** — Once Claude runs `git push` or deletes files, you can't undo easily

Good prompt engineering isn't about "tricking" Claude or writing longer prompts — it's about providing complete specifications that a professional developer would need.

## The Anatomy of an Effective Prompt

An effective Claude Code prompt has four components:

```
[CONTEXT] + [GOAL] + [CONSTRAINTS] + [ACCEPTANCE CRITERIA]
```

### 1. Context: What Claude Needs to Know

**What to include:**
- Technology stack (language, framework, versions)
- Relevant codebase structure
- Existing patterns or conventions
- Current state or problem

**Example:**
```
I'm building a React 18 app with TypeScript and React Router v6.
We use Tailwind for styling and Zustand for state management.
Authentication is currently hardcoded; users are stored in localStorage.
```

### 2. Goal: What You Want to Achieve

**What to include:**
- The specific outcome you need
- The problem you're solving
- Why this matters (helps Claude make tradeoffs)

**Example:**
```
I need to replace the hardcoded authentication with a proper JWT-based system
that validates tokens on the backend. This will allow us to scale to multiple
users and add role-based permissions later.
```

### 3. Constraints: Boundaries and Requirements

**What to include:**
- Technologies to use or avoid
- Files to modify or leave alone
- Performance requirements
- Security requirements
- Backward compatibility needs

**Example:**
```
Requirements:
- Use jsonwebtoken library for JWT handling
- Store tokens in httpOnly cookies (not localStorage)
- Don't modify the existing user registration flow
- Keep the current UI components; only change the auth logic
- Tokens should expire after 24 hours
```

### 4. Acceptance Criteria: How to Know It's Done

**What to include:**
- Testable outcomes
- Expected behavior
- Edge cases to handle

**Example:**
```
Success looks like:
- Users can log in and receive a JWT
- Protected routes redirect unauthenticated users to /login
- Token refresh happens automatically before expiration
- Logout clears the token and redirects to home
- Invalid tokens return 401 and force re-login
```

## Practical Examples

### Basic Usage: Simple Feature Addition

❌ **Bad:**
```
Add a dark mode toggle
```

**Problems:**
- Where should the toggle go?
- How should it persist?
- What colors should change?
- Should it be in settings or always visible?

✅ **Good:**
```
Add a dark mode toggle to the header navigation bar (next to the user avatar).

Context:
- We're using React 18 with Tailwind CSS
- The theme should persist across sessions using localStorage
- We already have a light theme defined

Requirements:
- Toggle should be a moon/sun icon that switches when clicked
- Use Tailwind's dark: modifiers for styling
- Update the <html> element's class to 'dark' when enabled
- Default to light mode for new users

The toggle should appear on all pages since the header is global.
```

### Intermediate Example: Refactoring Existing Code

❌ **Bad:**
```
Refactor the API calls to use async/await
```

**Problems:**
- Which API calls? (There might be dozens)
- Should error handling change?
- Are there breaking changes to consider?

✅ **Good:**
```
Refactor the authentication API calls in src/services/auth.ts to use
async/await instead of Promise chains.

Current situation:
- We have login(), register(), and logout() functions using .then()/.catch()
- These functions are called from React components using .then()
- Error handling currently shows alerts

Changes needed:
- Convert the three auth functions to async/await
- Update error handling to throw errors instead of alerting
- Update the components that call these (LoginForm.tsx, RegisterForm.tsx)
- Wrap calls in try/catch blocks with proper error state management

Don't touch:
- Other API calls in src/services/ (we'll do those later)
- The API response format
- The current test suite

The goal is cleaner code without changing behavior.
```

### Advanced Usage: Complex Multi-File Feature

❌ **Bad:**
```
Build a notification system
```

**Problems:**
- What kind of notifications? (Toast, modal, bell icon, push?)
- Backend or frontend only?
- Real-time or polling?
- What events trigger notifications?

✅ **Good:**
```
Implement a real-time notification system using WebSockets.

Context:
- Next.js 14 app (App Router) with TypeScript
- Backend is Express.js with Socket.io already installed
- We need to notify users when they receive new messages or mentions

Architecture:
- Frontend: Toast notifications (use react-hot-toast library)
- Backend: Socket.io connection per authenticated user
- Notifications should persist: store in PostgreSQL notifications table
- Bell icon in header shows unread count

Implementation plan:
1. Create database schema: notifications table with user_id, message, type, read_at
2. Backend: Socket.io setup with JWT authentication
3. Backend: Emit events on message/mention actions
4. Frontend: Socket.io client connection (only when authenticated)
5. Frontend: Toast component for real-time popups
6. Frontend: Notification center dropdown (click bell icon)

Requirements:
- Reconnect automatically if connection drops
- Mark notifications as read when clicked
- Show max 5 toasts at once (queue the rest)
- Notification center shows last 20 notifications
- Include "Mark all as read" button

Don't include:
- Email notifications (future work)
- Push notifications (future work)
- Sound/vibration (might add later)

Success criteria:
- Sending a message triggers a toast for the recipient
- Notifications persist after refresh
- Unread count updates in real-time
- No memory leaks from unclosed sockets
```

## Common Patterns

### Pattern 1: Exploratory First, Then Specific

When you don't know the codebase well:

> [!tip] Explore Then Act
> Ask Claude to explore first, then craft a specific request based on what it finds.

```
Step 1: "Search the codebase for how we currently handle user authentication.
        Show me the relevant files and how they're structured."

[Claude explores and reports back]

Step 2: "Thanks! Now update the authentication in src/services/auth.ts to use
        JWT tokens instead of the current session-based approach. Keep the same
        API endpoints but change the token handling..."
```

### Pattern 2: Specify Outcomes, Not Implementation

Let Claude choose the best approach:

> [!tip] What, Not How
> Specify the outcome you need, not every step to get there.

❌ **Over-specified:**
```
Create a function called getUserById that takes an id parameter, queries
the database using a SELECT statement with a WHERE clause on the id column,
returns the result as a User object, and handles errors with a try/catch block.
```

✅ **Outcome-focused:**
```
Create a function that retrieves a user by ID from the database.
It should return a User object or null if not found, and handle errors gracefully.

Note: We're using Prisma ORM, so follow existing patterns in src/db/queries.ts
```

Claude will use the appropriate Prisma method, follow existing error handling patterns, and write idiomatic code.

### Pattern 3: Iterative Refinement

For complex tasks, work in stages:

> [!tip] Break It Down
> Large tasks benefit from step-by-step refinement with validation points.

```
Phase 1: "Create the database schema for a notification system. Show me the
         schema before creating any migrations."

[Review schema]

Phase 2: "Looks good! Now generate the migration and create the Prisma model."

[Verify migration]

Phase 3: "Now implement the Socket.io connection logic on the backend."

[Test connection]

Phase 4: "Now add the frontend notification component..."
```

This prevents cascading errors and lets you catch issues early.

### Pattern 4: Provide Examples

Show Claude what "good" looks like:

> [!tip] Examples as Specifications
> Point to existing code that Claude should match or emulate.

```
Add error handling to the new API endpoints following the same pattern
used in src/api/users.ts lines 45-60.

I want the same structure: try/catch blocks, logging with Winston,
and consistent error response format { error: string, code: number }.
```

This is more precise than describing the pattern in words.

## Common Anti-Patterns

### Anti-Pattern 1: Assuming Claude Knows Your Codebase

❌ **Assumes too much:**
```
Add validation to the form
```

**Problems:**
- Which form? (You might have dozens)
- What kind of validation?
- Where should errors display?

✅ **Provides context:**
```
Add client-side validation to the LoginForm component (src/components/LoginForm.tsx).

Validate:
- Email: must be valid email format
- Password: minimum 8 characters

Show error messages below each field in red text when user blurs the input.
We're using react-hook-form, so use its validation API.
```

### Anti-Pattern 2: Vague Modifiers

❌ **Vague words:**
```
Make the API faster
Improve the UI
Fix the bugs
Clean up the code
```

**Problems:** "Faster" could mean caching, optimization, different algorithm, or CDN. Each word has dozens of interpretations.

✅ **Specific requests:**
```
The /api/users endpoint is taking 2-3 seconds to respond.
Add pagination (20 users per page) to reduce the query size.
Also add an index on the email column since we filter by it.
```

### Anti-Pattern 3: Hidden Assumptions

❌ **Implicit requirements:**
```
Add a contact form
```

**Hidden assumptions:**
- It should email someone
- It needs spam protection
- It has specific fields
- It has validation
- It needs to be styled

✅ **Explicit requirements:**
```
Add a contact form to the /contact page with these fields:
- Name (required, text input)
- Email (required, email input with validation)
- Message (required, textarea, max 1000 chars)

On submit:
- Show loading spinner
- Send data to /api/contact endpoint (create this)
- Show success message or error
- Clear form on success

Backend should:
- Send email to admin@example.com using Nodemailer
- Save submission to database
- Rate limit: 3 submissions per IP per hour

Style to match the existing form components in src/components/forms/
```

### Anti-Pattern 4: No Boundaries

❌ **Open-ended:**
```
Improve the authentication system
```

**Problems:**
- Does this mean add OAuth? 2FA? Password reset? Session management?
- Which files can be modified?
- What shouldn't change?

✅ **Clear boundaries:**
```
Add password reset functionality to the existing authentication system.

Add:
- "Forgot password?" link on login page
- Password reset request form (enter email)
- Email with reset token (expires in 1 hour)
- Reset password form (token + new password)

Don't change:
- Current login/register flows
- JWT token implementation
- User model schema

Use the same email service we're using for registration emails (Sendgrid).
```

## Edge Cases & Gotchas

### Gotcha 1: Claude Takes You Literally

**Problem:** Claude implements exactly what you said, even if it's not what you meant.

**Example:**
```
You: "Remove all console.log statements"
Claude: *Removes all console.log, including important debugging in error handlers*
```

**Fix:** Be specific about scope.
```
"Remove console.log statements used for debugging, but keep the ones in error
 handlers and the startup logging in server.ts"
```

### Gotcha 2: Underspecified Edge Cases

**Problem:** Claude handles the happy path perfectly but ignores edge cases you didn't mention.

**Example:**
```
You: "Add a search function"
Claude: *Adds search but doesn't handle empty queries, special characters, or no results*
```

**Fix:** Call out edge cases explicitly.
```
"Add a search function that:
- Shows 'Start typing to search' when query is empty
- Escapes special characters in the query
- Shows 'No results found' message when there are no matches
- Debounces input by 300ms to avoid excessive API calls"
```

### Gotcha 3: Conflicting Requirements

**Problem:** You provide requirements that contradict each other.

**Example:**
```
"Make it fast" + "Add comprehensive logging" + "Support offline mode"
```

These can conflict (logging slows things down, offline mode complicates caching).

**Fix:** Prioritize requirements.
```
"Priority 1: Must work offline
 Priority 2: Should be fast (< 100ms response)
 Priority 3: Nice to have: detailed logging

If logging impacts performance, make it optional via debug flag."
```

### Gotcha 4: Technology Mismatches

**Problem:** Requesting features incompatible with your stack.

**Example:**
```
You: "Use Redux for state management"
Your app: Uses React Server Components (incompatible with Redux)
```

**Fix:** Let Claude validate approach or state your stack clearly.
```
"We're using Next.js 14 with Server Components. What's the best way to handle
 global state for user preferences? If Redux doesn't work, suggest alternatives."
```

## Debugging Your Prompts

When Claude's output isn't what you expected, check:

1. **Did I provide enough context?**
   - Stack, framework versions, relevant files?

2. **Was my goal specific?**
   - "Add auth" vs "Add JWT-based authentication with email/password"?

3. **Did I set boundaries?**
   - What should/shouldn't change?

4. **Did I include examples?**
   - Can I point to existing code Claude should match?

5. **Are there hidden assumptions?**
   - Did I assume Claude knows my conventions?

6. **Is this actually multiple requests?**
   - Should I break this into smaller steps?

## The 5-Second Test

Before hitting enter, read your prompt and ask:

> "If I gave this to a senior developer who just joined the team,
> would they have enough information to implement it correctly?"

If not, add more detail.

## Advanced: Meta-Prompting

You can ask Claude to help craft better prompts:

```
I want to add a feature but I'm not sure how to explain it clearly.

The feature is: [rough description]

Can you help me structure this into a clear request with:
- Necessary context
- Specific requirements
- Clear boundaries
- Success criteria

Ask me questions if you need more details to craft a good prompt.
```

This is especially useful for complex features where you're not sure what information matters.

## Measuring Prompt Quality

Your prompts are improving when:

✅ Claude rarely asks for clarification
✅ First implementations match your intent
✅ You can predict what Claude will do
✅ Less back-and-forth to get desired results
✅ Fewer bugs or misunderstandings

## Related Topics

- [[Chain-of-Thought]] — How Claude reasons through problems
- [[Few-Shot-Prompting]] — Using examples to guide behavior
- [[Context-Management-Strategies]] — Providing the right information at the right time
- [[Claude-Code-Best-Practices]] — Hub document for all best practices

## References

- [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Claude Documentation](https://docs.anthropic.com/en/docs/)
- [Prompt Engineering Techniques](https://www.promptingguide.ai/)
- [Best Practices for Claude Code](https://github.com/anthropics/claude-code)