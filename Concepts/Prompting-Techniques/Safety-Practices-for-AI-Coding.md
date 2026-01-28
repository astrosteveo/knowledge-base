---
title: Safety Practices for AI Coding
category: Concepts/Prompting-Techniques
tags:
  - claude-code
  - safety
  - security
  - destructive-operations
  - validation
difficulty: beginner
status: evergreen
date-created: 2026-01-13
date-updated: 2026-01-13
sources:
  - https://docs.anthropic.com/en/docs/
  - https://github.com/anthropics/claude-code
---

> [!summary]
> Learn to prevent destructive operations, establish safety boundaries, and implement validation patterns that protect your codebase while working with Claude Code. Understand why dangerous commands happen, how to prevent them, and how to build safe, confident AI-assisted development workflows.

## What Are Safety Practices?

Safety practices are patterns and conventions that prevent unintended destructive operations when working with AI coding assistants. These include:

- **Permission boundaries** — What Claude can and cannot do
- **Validation patterns** — Checks before destructive operations
- **Clear communication** — Removing ambiguity about sensitive actions
- **Rollback strategies** — How to undo mistakes safely
- **Testing protocols** — Verifying changes before they're permanent

**Critical Understanding:** Claude doesn't know what's dangerous in your specific context. It relies on clear instructions, permission prompts, and established patterns to operate safely.

## Why Dangerous Operations Happen

### The `rm -rf .` Problem

The classic example: Claude runs `rm -rf .` and deletes your entire directory.

**Why it happens:**

```mermaid
flowchart TD
    A[User: "Clean up the project"] --> B{Claude interprets}
    B --> C[Remove unnecessary files?]
    B --> D[Delete build artifacts?]
    B --> E[Reset to clean state?]

    C --> F{What's unnecessary?}
    D --> G{What are artifacts?}
    E --> H{What's a clean state?}

    F --> I[Claude guesses]
    G --> I
    H --> I

    I --> J[Chooses broad interpretation]
    J --> K[rm -rf . seems to match]
```

**Root cause:** Ambiguous instruction + no explicit boundaries + assumption that "clean" might mean "delete everything."

### Other Common Dangerous Operations

1. **Force pushing to main**
   - User: "Push the changes"
   - Claude: `git push --force origin main`
   - Why: Interprets urgency, doesn't know main is protected

2. **Dropping production database**
   - User: "Reset the database"
   - Claude: `DROP DATABASE production;`
   - Why: Doesn't distinguish dev from prod without context

3. **Deleting node_modules without backup**
   - User: "Fix the dependency issues"
   - Claude: `rm -rf node_modules package-lock.json`
   - Why: Common solution pattern, but may break working setup

4. **Modifying all files matching a pattern**
   - User: "Update all imports"
   - Claude: [Changes 500 files including node_modules]
   - Why: "All" means literally all without boundaries

5. **Committing sensitive files**
   - User: "Commit these changes"
   - Claude: `git add . && git commit` [includes .env with secrets]
   - Why: Doesn't have concept of "sensitive" without instruction

## Practical Examples

### Basic Usage: Safe Cleanup Request

❌ **Dangerous (vague):**
```
"Clean up the project"
```

**What could happen:**
- Deletes important files
- Removes configuration
- Deletes git history
- Removes dependencies

✅ **Safe (specific):**
```
"Remove generated files from the build/ and dist/ directories.
 Don't delete anything else, and show me the list of files
 before deleting them."
```

**Why it's safe:**
- Specific directories mentioned
- Excludes everything else explicitly
- Requires confirmation before deletion

### Intermediate Example: Database Operations

❌ **Dangerous (no environment context):**
```
"Reset the database to fix the migration issue"
```

**What could happen:**
- Drops production database
- Loses real user data
- Breaks running application

✅ **Safe (with environment and boundaries):**
```
"I'm working in the LOCAL development environment.
 Reset the local SQLite database (data/dev.db) by:
 1. Backing up current db to data/dev.db.backup
 2. Running migrations from scratch
 3. Seeding with test data

 Don't touch any other database files or remote databases.
 Show me the commands before running them."
```

**Why it's safe:**
- Explicitly states environment (LOCAL)
- Specific file mentioned
- Backup step included
- Requires preview before execution

### Advanced Usage: Git Operations

❌ **Dangerous (implicit force):**
```
"Push my changes, they're important"
```

**What could happen:**
- Force push overwriting remote
- Push to wrong branch
- Push secrets in commit history
- Break CI/CD pipeline

✅ **Safe (explicit process):**
```
"Before pushing, help me:
 1. Show git status and current branch
 2. Verify no sensitive files are staged (.env, keys, tokens)
 3. Show the diff of what will be pushed
 4. Confirm the target branch is 'feature/add-auth' (NOT main)

 Then push to origin feature/add-auth (no force push).
 If push is rejected, tell me why instead of forcing."
```

**Why it's safe:**
- Multi-step verification
- Explicit no-force-push instruction
- Branch verification
- Secret detection
- Informational instead of automatic force

## Common Patterns

### Pattern 1: Dry Run First

> [!tip] Preview Before Execute
> For destructive operations, always request preview or dry run.

```
"Find all console.log statements in src/ and show me the list.
 Don't delete anything yet—I want to review first."

[Review list]

"Okay, remove only the ones in src/components/ (not src/utils/)."
```

**Applies to:**
- File deletion
- Mass find-and-replace
- Database operations
- Git history rewrites

### Pattern 2: Backup Before Modify

> [!tip] Create Safety Nets
> Request explicit backups before risky changes.

```
"Before modifying the database schema:
 1. Export current schema to migrations/backup-2026-01-13.sql
 2. Create a database snapshot
 3. Then run the ALTER TABLE commands"
```

**Applies to:**
- Database modifications
- Large-scale refactoring
- Config file changes
- Critical system files

### Pattern 3: Explicit Boundaries

> [!tip] State What's Off-Limits
> Define protected resources explicitly.

```
"Update all TypeScript files in src/components/ to use named exports.

Protected files (DON'T TOUCH):
- src/legacy/ (will break old code)
- src/lib/ (external libraries)
- node_modules/ (obviously)
- Anything outside src/components/

Ask me before modifying more than 10 files."
```

**Applies to:**
- Mass operations
- Find-and-replace
- Refactoring
- Dependency updates

### Pattern 4: Confirmation Gates

> [!tip] Require Approval at Key Points
> Build checkpoints into complex operations.

```
"I need to migrate from Redux to Zustand.

Step 1: Show me the migration plan with affected files.
[Wait for approval]

Step 2: Migrate one store as a proof-of-concept.
[Wait for testing]

Step 3: Only after I confirm, migrate the rest."
```

**Applies to:**
- Major refactorings
- Architecture changes
- Breaking changes
- Production deployments

### Pattern 5: Environment Awareness

> [!tip] Always Specify Environment
> State explicitly which environment you're working in.

```
"In my LOCAL development environment (not staging, not production):
 Seed the database with 100 test users.

 Verify you're targeting localhost:5432/myapp_dev before running."
```

**Applies to:**
- Database operations
- API testing
- Deployment commands
- Environment-specific config

## Git Safety Protocol

Git operations are particularly dangerous. Follow this protocol:

### Never Skip Without Permission

Claude has built-in git safety rules:

> [!warning] Git Safety Rules
> - NEVER update git config
> - NEVER run destructive commands (force push, hard reset) unless explicitly requested
> - NEVER skip hooks (--no-verify) unless explicitly requested
> - NEVER force push to main/master
> - NEVER amend commits that have been pushed
> - ALWAYS verify branch before pushing

### Safe Commit Pattern

```bash
# 1. Show status first
git status

# 2. Review changes
git diff

# 3. Stage specific files (avoid 'git add .')
git add src/components/Header.tsx
git add src/styles/header.css

# 4. Commit with descriptive message
git commit -m "Add mobile navigation to Header component

- Implement hamburger menu for mobile viewports
- Add responsive styles for <768px
- Update Header tests

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"

# 5. Push to correct branch (no force)
git push origin feature/mobile-nav
```

### When Commits Fail

If pre-commit hooks fail or commit is rejected:

❌ **Dangerous:**
```bash
git commit --no-verify  # Skips safety checks
git push --force        # Overwrites remote
```

✅ **Safe:**
```
"The pre-commit hook failed with: [error message]
 Help me fix the issue and create a NEW commit (don't amend or skip hooks)."
```

## Permission Patterns

### Requesting Permissions Upfront

In plan mode, you can request permission categories:

```
"I need to implement user authentication.

This will require:
- Running tests (npm test)
- Installing dependencies (npm install jsonwebtoken bcrypt)
- Running database migrations (npx prisma migrate dev)
- Restarting the dev server

Do you approve these categories of operations?"
```

Claude can then execute without asking each time.

### Implicit vs. Explicit Permissions

**Implicit (risky):**
```
User: "Fix the bug"
Claude: [Modifies 5 files, runs tests, commits, pushes]
```

**Explicit (safe):**
```
User: "Fix the bug in auth.ts. After fixing, run tests.
      If tests pass, commit the change but DON'T push yet."
```

> [!tip] Default to Explicit
> When in doubt, be explicit about what operations are allowed.

## Validation Checklists

### Before Deleting Files

- [ ] Specific files/directories mentioned (not "cleanup everything")
- [ ] Files listed for review before deletion
- [ ] No wildcard patterns without boundaries (`*` → `src/**/*.log`)
- [ ] Backup created if needed
- [ ] Git tracked files won't be lost (committed or stashed)

### Before Git Operations

- [ ] Current branch verified (not main/master for risky ops)
- [ ] No sensitive files staged (.env, keys, credentials)
- [ ] Commit message is descriptive
- [ ] Push target branch is correct
- [ ] No --force flag unless explicitly needed and approved
- [ ] Pre-commit hooks will run (no --no-verify)

### Before Database Operations

- [ ] Environment explicitly stated (local/dev/staging/prod)
- [ ] Backup created or verified
- [ ] Connection string checked
- [ ] Dry-run performed if available
- [ ] Rollback plan exists

### Before Mass Operations

- [ ] Scope clearly defined (which directories/files)
- [ ] Protected areas excluded
- [ ] Count of affected items shown
- [ ] Sample shown before executing
- [ ] Confirmation required if count > threshold (e.g., 20 files)

## Edge Cases & Gotchas

### Gotcha 1: "Clean" Means Different Things

**Problem:** Words like "clean," "reset," "fresh," "fix" are ambiguous.

❌ **Ambiguous:**
```
"Clean up the database"
```

Could mean:
- Delete old records
- Drop and recreate
- Vacuum and optimize
- Remove test data
- Reset to empty state

✅ **Unambiguous:**
```
"Delete records from the 'sessions' table where createdAt is older than 7 days.
 Keep all other tables and data intact."
```

### Gotcha 2: Claude Takes Urgency Literally

**Problem:** Urgent language can trigger aggressive solutions.

❌ **Urgency without boundaries:**
```
"This is urgent, just push the fix to production now!"
```

Claude might:
- Skip tests
- Skip review
- Force push
- Skip backups

✅ **Urgent with safety:**
```
"This is urgent, but we still need to follow the deployment process:
 1. Run tests locally
 2. Push to staging
 3. Verify on staging
 4. Then deploy to production via CI/CD

Help me do this quickly but safely."
```

### Gotcha 3: Implicit "All"

**Problem:** Operations without scope apply to everything Claude can access.

❌ **No scope:**
```
"Update the imports to use absolute paths"
```

Claude might update:
- Your code
- node_modules
- Test fixtures
- Generated code
- Documentation examples

✅ **Explicit scope:**
```
"Update imports to absolute paths in these directories only:
 - src/components/
 - src/pages/
 - src/hooks/

 Exclude: src/legacy/, node_modules/, src/__tests__/__fixtures__/"
```

### Gotcha 4: Production Context Confusion

**Problem:** Not distinguishing between environments.

❌ **No environment:**
```
"Connect to the database and check user count"
```

**Risk:** Might connect to production instead of development.

✅ **Environment specified:**
```
"Connect to my LOCAL development database (DATABASE_URL from .env.local)
 and count users. Do not connect to any remote database."
```

## Rollback Strategies

When things go wrong:

### File Deletion Recovery

If files were accidentally deleted:

```bash
# If committed
git checkout HEAD -- <file-path>

# If staged but not committed
git restore --staged <file-path>
git restore <file-path>

# If not in git but recently deleted (Linux/Mac)
# Check trash/bin if available
```

### Git History Recovery

If commits were force pushed:

```bash
# Find lost commit
git reflog

# Restore to that commit
git reset --hard <commit-hash>

# If branch was deleted
git checkout -b <branch-name> <commit-hash>
```

### Database Recovery

If database was modified:

```bash
# Restore from backup
pg_restore -d myapp_dev backup.sql

# Or undo specific migration
npx prisma migrate resolve --rolled-back <migration-name>
```

### Prevention: Version Control Everything

> [!tip] Git Is Your Safety Net
> Commit working states frequently. It's easier to undo with git than without.

```
Before making risky changes:
1. git add -A
2. git commit -m "WIP: Before risky refactoring"
3. [Make changes]
4. If it works: git commit --amend
5. If it fails: git reset --hard HEAD
```

## Testing in Safe Environments

### Use Development Containers

```
"Work in the Docker development container (not my host system).
 Run all destructive commands there first."
```

### Create Test Branches

```
"Create a test branch called 'experiment/unsafe-refactor' and work there.
 If it works, we'll merge. If not, we'll delete the branch."
```

### Use Staging Databases

```
"Connect to the staging database (staging.myapp.com:5432) to test
 this migration. Once verified, we'll run on production."
```

## Measuring Safety

Your workflow is safe when:

✅ You feel confident letting Claude make changes
✅ Destructive operations always have preview steps
✅ You have backups or version control for everything
✅ No "oh shit" moments where something unexpected breaks
✅ Rollback is easy and practiced

Your workflow needs improvement if:

❌ You're nervous about what Claude will do
❌ You've had files deleted accidentally
❌ Force pushes have caused problems
❌ You don't have backups of important data
❌ You avoid certain operations because they feel risky

## Quick Safety Checklist

Before approving any Claude Code operation, ask:

- [ ] Is this operation reversible? If not, do I have a backup?
- [ ] Have I specified the exact scope (files, directories, environment)?
- [ ] Are there sensitive files that could be accidentally exposed?
- [ ] Did I request preview/dry-run for destructive operations?
- [ ] Is the target environment clearly stated (local/staging/prod)?
- [ ] Would I be comfortable if a junior developer did this?

If any answer is uncertain, add more safety boundaries.

## Related Topics

- [[Prompt-Engineering-for-Claude-Code]] — Clear communication prevents accidents
- [[Context-Management-Strategies]] — Proper context helps Claude understand safety
- [[Debugging-Claude-Code-Outputs]] — Recovering from mistakes
- [[Claude-Code-Best-Practices]] — Overall best practices hub

## References

- [Anthropic Safety Best Practices](https://docs.anthropic.com/en/docs/)
- [Git Safety Documentation](https://git-scm.com/docs)
- [Claude Code Safety Guidelines](https://github.com/anthropics/claude-code)