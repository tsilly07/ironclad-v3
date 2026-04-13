---
name: root-cause-isolation
description: Use when something is broken — isolates the actual cause using binary search on assumptions, not sequential guessing. Fastest path from symptom to root cause.
---

# Root Cause Isolation

**Core principle:** Every symptom has exactly one root cause. Find the cause, not the symptom.

---

## The Problem With Standard Debugging

Standard debugging: look at the error, try a fix, see if it helps, repeat.  
This is O(n) over assumptions — you check one at a time, randomly.

Root cause isolation uses **binary search** — eliminating half the assumption space per step.

---

## Step 1 — Symptom vs Cause Separation

State the symptom. Then explicitly separate it from possible causes.

```markdown
Symptom: "The API returns 500 on POST /users"

Not the cause (yet):
- The database is broken
- The auth middleware is failing
- The request body is wrong
- The route handler has a bug
- The validation is throwing

These are HYPOTHESES. Not causes. Not yet.
```

The symptom is what you observe. The cause is what produces it. Never confuse them.

---

## Step 2 — Build the Assumption Space

List every assumption the system is making when the symptom occurs.

For each component in the call chain:
- What does this component assume about its inputs?
- What does it assume about its environment?
- What does it assume about what comes before it?

```markdown
Call chain: Request → Auth middleware → Validator → Route handler → DB query

Assumptions:
- Auth: token is in headers, format is Bearer {jwt}
- Validator: body is JSON, fields match schema
- Handler: DB connection is available
- DB query: table exists, user has write permission
```

---

## Step 3 — Binary Search

Identify the midpoint of the call chain. Test there first.

```
If midpoint passes → root cause is in the second half
If midpoint fails → root cause is in the first half
Repeat until isolated to a single component
```

**Implementation:**
- Add a log/probe at the midpoint
- Run the failing case
- Read the output
- Eliminate half the assumption space

Repeat 3–4 times. You'll have the root cause.

---

## Step 4 — Cause Validation

When you have a hypothesis: prove it before fixing it.

Write a minimal reproduction:
```
Minimum reproduction: the smallest possible code that exhibits the symptom.
If you can't write a minimal reproduction: you don't fully understand the cause.
```

If minimal repro succeeds → you have the cause. Now fix it.  
If minimal repro doesn't reproduce the symptom → your hypothesis is wrong. Back to Step 3.

---

## Step 5 — Root Cause Statement

Before writing any fix, write a clear root cause statement:

```markdown
## Root Cause

The `POST /users` endpoint returns 500 because the `validateBody` middleware
throws an uncaught exception when the `role` field is missing — the validator
expects a default value of "user" but only sets it after schema validation runs,
which throws before the default is applied.

Evidence: Minimal repro confirms the issue exists. Line 47 of validator.ts.
```

If you can't write this statement clearly: you don't fully understand the cause yet.

---

## Step 6 — Fix and Verify

Fix the root cause — not the symptom.

```
Wrong fix: Add try/catch around the validator call → hides the bug
Right fix: Set the default value before validation runs → eliminates the bug
```

After the fix:
- Re-run the minimal repro → symptom gone ✅
- Run the full test suite → nothing broken ✅
- Verify against failure-modes log → this mode is now mitigated ✅

---

## Anti-Patterns That Waste Time

| Anti-Pattern | Why It's Wrong |
|---|---|
| "Let me try X and see if it helps" | Testing fixes without understanding cause → lucky accident, not understanding |
| Fixing the symptom | The same root cause produces other symptoms you haven't found yet |
| Reading all the code | Debugging by searching is O(n). Binary search is O(log n). |
| "This should work" | Your expectation is the assumption. Test the assumption, not the code. |
| Adding more logging first | Log at the midpoint of the call chain. Then narrow. Not everywhere. |

---

## Time Budget

If you've spent more than 20 minutes on a bug without isolating the root cause to a specific line or function: stop. Re-run the binary search from the top. You've gone down a wrong branch.

---

## Red Flags

- Same fix attempted twice → you don't understand the cause
- 3 different fixes, 3 different errors → cascade, not root cause
- "It's complicated" → Complexity is a sign you haven't isolated yet
- "It just started happening" → State changed. Find what changed. Check git log.
