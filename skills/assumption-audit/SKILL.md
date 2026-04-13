---
name: assumption-audit
description: Use before any implementation begins — surfaces, rates, and validates every assumption the agent is making. Any assumption under 80% confidence must be resolved before proceeding. Prevents "works on my machine" and "I thought you meant" failures.
---

# Assumption Audit

**Core principle:** Every implementation failure has a root cause of: wrong assumption, unchecked assumption, or assumption never questioned.

---

## When to Use

- Before writing any code
- Before executing any task in the task-graph
- When a task is failing in unexpected ways (retroactive audit)
- When confidence < 60% on any aspect of the current work

---

## The Five Assumption Categories

Audit across all five before proceeding:

### 1. Environment Assumptions
What are you assuming about where this code will run?

```
Examples:
- "This is a Node.js project" (confidence: ?)
- "The database is PostgreSQL" (confidence: ?)
- "This runs on Linux in production" (confidence: ?)
- "Dependencies are installed" (confidence: ?)
```

### 2. Interface Assumptions
What are you assuming about how this code connects to other things?

```
Examples:
- "The API returns JSON" (confidence: ?)
- "This function will be called with validated input" (confidence: ?)
- "The event fires before the timeout" (confidence: ?)
```

### 3. State Assumptions
What are you assuming about the state of the system when your code runs?

```
Examples:
- "The user is authenticated" (confidence: ?)
- "The config file is populated" (confidence: ?)
- "No concurrent writes will happen" (confidence: ?)
```

### 4. Behavioral Assumptions
What are you assuming other code or systems will do?

```
Examples:
- "The framework handles error propagation" (confidence: ?)
- "The cache will be warm on first call" (confidence: ?)
- "Retries are handled upstream" (confidence: ?)
```

### 5. Requirements Assumptions
What are you assuming about what the user actually wants?

```
Examples:
- "They want this to be backward compatible" (confidence: ?)
- "Performance is more important than readability here" (confidence: ?)
- "They want tests for this" (confidence: ?)
```

---

## Confidence Rating Protocol

Rate each assumption on a 0–100% scale:

| Score | Meaning | Action |
|---|---|---|
| 90–100% | Verified fact — you can prove it | Proceed |
| 80–89% | High confidence — strong evidence | Proceed, document assumption |
| 60–79% | Medium confidence — likely but unverified | Validate before proceeding |
| 40–59% | Low confidence — guess with some basis | Must validate |
| 0–39% | Speculation | Stop. Ask immediately. |

**Rule:** No assumption below 80% may remain unvalidated before implementation begins.

---

## Validation Methods

For each assumption below 80%:

| Method | When to Use |
|---|---|
| Read the file directly | Environment, config, interface questions |
| Run a command | State verification |
| Ask the user | Requirements, priorities, unstated preferences |
| Check docs/tests | Behavioral assumptions |
| Write a probe | Can't read — must observe |

---

## Assumption Log

Document assumptions in `docs/ironclad/assumptions/YYYY-MM-DD-<task>-assumptions.md`:

```markdown
## Assumption Log — [Task Name]

| ID | Assumption | Category | Confidence | Validation | Status |
|---|---|---|---|---|---|
| A-01 | Project uses TypeScript | Environment | 95% | tsconfig.json exists | ✅ Verified |
| A-02 | Auth middleware handles token refresh | Behavioral | 65% | Read auth.middleware.ts | 🔄 Pending |
| A-03 | Tests should cover edge cases | Requirements | 50% | Asked user | ❌ Needs answer |
```

---

## Retroactive Audit (When Things Go Wrong)

If code is failing or behavior is unexpected, run an assumption audit retroactively:

1. List the top 5 assumptions you made during implementation
2. Rate them honestly (not what you want them to be — what they are)
3. Find the assumption that broke
4. Feed into `root-cause-isolation`

**Rule:** Before blaming the framework, the language, or the runtime — audit your assumptions.

---

## Red Flags

- "This is obvious" → Obvious things are unexamined assumptions. Rate it.
- "I'm pretty sure this is right" → "Pretty sure" = 70%. Document and validate.
- "The user would have mentioned it if it mattered" → That's an assumption too.
- "I'll check later if it breaks" → Check now. Later costs more.
