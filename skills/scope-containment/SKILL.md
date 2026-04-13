---
name: scope-containment
description: Use in real-time during any task — detects when work is growing beyond its original boundary, forces a reclassification decision, and prevents scope creep from compounding silently.
---

# Scope Containment

**Core principle:** Every uncontained scope expansion is a commitment made without approval.

---

## The Problem

Scope creep doesn't feel like a decision. It feels like "while I'm here, I might as well..." Every individual expansion seems reasonable. Collectively, they destroy timelines, budgets, and focus.

This skill makes scope creep visible and explicit — so it becomes a choice, not an accident.

---

## When to Use

This skill is **always active** during execution. It is not invoked explicitly — it runs as a continuous check.

Invoke it explicitly when you notice:
- A task is taking longer than its complexity rating suggested
- You've opened files outside the original task's scope
- A fix requires changing more than 2 files not in the original plan
- You're solving a problem that wasn't in the task-graph
- "While I'm here" has appeared in your reasoning more than once

---

## The Scope Boundary

At the start of each task, the scope boundary is defined by:
1. Files listed in the task-graph task definition
2. Functions/modules identified in the design doc
3. Test criteria in the task definition

**Anything outside this boundary is out of scope — explicitly.**

---

## Real-Time Scope Check

Run this check whenever you're about to take an action:

```
Question: "Is this action within the defined scope of the current task?"

If YES → proceed
If NO → trigger scope containment protocol
If UNCERTAIN → treat as NO
```

---

## Scope Containment Protocol

When an out-of-scope action is detected:

### Step 1 — Stop and Surface

Do not make the change. State explicitly:

```
"I've identified something outside the current task scope:
[Description of discovered issue/improvement]

This is NOT in the current task definition. Options:
A) Ignore it for now, log as future work
B) Add it to the task-graph as a new task
C) Expand current task scope (requires approval)
D) Treat it as a blocker — current task can't complete without it"
```

### Step 2 — Classify the Discovery

| Type | Definition | Default Action |
|---|---|---|
| **Unrelated improvement** | Nice to have, current task works without it | Log, ignore |
| **Adjacent bug** | Bug in nearby code, doesn't affect current task | Log as separate task |
| **Required dependency** | Current task actually can't work without this | Expand scope (get approval) |
| **Blocker** | Can't proceed at all without resolving this | Escalate immediately |

### Step 3 — Act Based on Classification

- **Log, ignore**: Add to `docs/ironclad/backlog/YYYY-MM-DD-scope-discoveries.md`. Move on.
- **New task**: Add to task-graph as a new task in a future phase. Move on.
- **Expand scope**: Get human approval. Update task-graph. Then proceed.
- **Blocker**: Stop execution. Escalate. Do not work around it silently.

---

## The Scope Creep Tax

Every undocumented scope expansion has a hidden tax:
- Time: you spent time on something you weren't asked to do
- Risk: more changes = more ways things can break
- Clarity: reviewers can't tell what belongs to which task
- Trust: human partner didn't approve this work

The tax compounds. Three small expansions = one large unreviewed change.

---

## Scope Discovery Log

Every out-of-scope discovery gets logged, even if action is "ignore":

```markdown
## Scope Discovery Log

| Date | Discovered During | Description | Classification | Action |
|---|---|---|---|---|
| 2025-01-15 | T-03 | Auth middleware has no rate limiting | Adjacent bug | Logged as T-08 |
| 2025-01-15 | T-03 | Logging format inconsistent in 3 files | Unrelated improvement | Ignored, backlog |
```

---

## Legitimate Scope Expansion

Sometimes scope must expand. This is fine — with approval.

A legitimate scope expansion:
1. Is surfaced explicitly (not done silently)
2. Gets a new task entry in the task-graph
3. Has a dependency relationship defined
4. Gets human confirmation before proceeding

It is not a failure to discover that scope must expand. It is a failure to expand scope without saying so.

---

## Red Flags

- "I'll just fix this quickly while I'm here" → You are expanding scope. Use the protocol.
- "This refactor is necessary for the feature to work" → Maybe. Classify it. Get approval.
- "It'll only take 5 minutes" → That's a scope expansion rationalization.
- "The human would want this fixed" → Ask. Don't assume.
