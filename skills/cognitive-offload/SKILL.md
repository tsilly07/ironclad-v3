---
name: cognitive-offload
description: Use when working on large projects, multi-session work, or when context is becoming full — maintains a structured external memory in .ironclad/memory/ that is queryable, typed, and richer than a flat state file.
---

# Cognitive Offload

**Core principle:** Your context window is finite. Your project memory should not be.

---

## The Problem With PROJECT_STATE.md

A flat state file forces everything into unstructured prose. Finding relevant context means reading the whole file. Nothing is typed. Nothing is queryable. New sessions start with the same cognitive load as the last one ended with.

Cognitive offload replaces it with structured, typed memory.

---

## Memory Structure

All memory lives in `.ironclad/memory/`:

```
.ironclad/memory/
├── index.md          ← always-loaded summary (Tier 1)
├── decisions/        ← architectural and design decisions
├── context/          ← per-feature/component context
├── assumptions/      ← live assumption log (feeds assumption-audit)
├── risks/            ← open risks and failure modes
├── backlog/          ← scope discoveries, deferred work
└── session/          ← current session state (cleared on new session)
```

---

## Tier System

### Tier 1 — Always Loaded (index.md only)

`index.md` is the only file loaded at session start. It contains:

```markdown
# Project Memory Index

## Current State
- Active task: T-05 (integration layer)
- Phase: 2 of 4
- Blocking issues: none
- Last session ended: 2025-01-15, after T-04 completed

## Quick Reference
- Stack: Node.js, TypeScript, PostgreSQL, Redis
- Auth: JWT, 24h expiry, refresh via /auth/refresh
- Key constraint: all writes must be idempotent

## Open Items
- Risk: F-03 race condition (not addressed) → risks/F-03.md
- Decision needed: cache TTL strategy → decisions/cache-ttl.md
- Assumption unvalidated: webhook format → assumptions/A-07.md
```

Max length: 50 lines. If it exceeds this, something from the previous session was too verbose.

### Tier 2 — Load on Demand

Load specific memory files when the current task requires them:
- Working on auth? → `context/auth.md`
- Hitting a risk? → `risks/F-xx.md`
- Need to remember a decision? → `decisions/relevant-decision.md`

Never load Tier 2 files preemptively. Load when needed.

### Tier 3 — Reference Only

Large documents, full specs, full decision logs. Load only a specific section when you need it. Summarize what you need into Tier 1 or 2, then release.

---

## Memory File Formats

### Decision Record (`decisions/`)

```markdown
# Decision: [Short Name]
Date: YYYY-MM-DD
Status: Active | Superseded | Under Review

## Context
What situation made this decision necessary?

## Decision
What was decided?

## Rationale
Why this, not alternatives?

## Consequences
What does this make easier? What does it make harder?

## Superseded by
[link if replaced]
```

### Risk Record (`risks/`)

```markdown
# Risk: [Short Name]
ID: F-xx
Status: Open | Mitigated | Accepted | Resolved

## Description
What is the risk?

## Likelihood / Impact
Likelihood: X%  Impact: Low / Med / High / Critical

## Tripwire
How will we detect this?

## Recovery Path
What do we do if it triggers?
```

### Context Record (`context/`)

```markdown
# Context: [Component/Feature Name]
Last updated: YYYY-MM-DD

## What It Does
## Key Files
## Known Quirks
## Integration Points
## Open Questions
```

---

## Session Start Protocol

At the start of any new session:
1. Load `index.md` only
2. Identify current task from index
3. Load only the context files for that task
4. Run assumption-audit on any Tier-2 assumptions before starting

Do not load everything. Do not re-read files from previous sessions unless the current task requires it.

---

## Session End Protocol

Before ending a session:
1. Update `index.md` with current state (5 minutes max)
2. Update any context files that changed
3. Log any new risks or decisions
4. Clear `session/` directory

---

## Red Flags

- "Let me re-read the entire codebase to get oriented" → Read index.md. Then load only what you need.
- "I'll remember this for the next session" → You won't. Write it to memory.
- "The state file is getting too long" → You're storing prose, not structured memory. Refactor into typed records.
- "I need to load all the docs first" → Load the index. Load only what you need from there.
