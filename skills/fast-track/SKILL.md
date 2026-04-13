---
name: fast-track
description: Use for isolated changes under 20 lines that don't affect system architecture, APIs, or shared state — bypasses the full workflow without bypassing quality. Not a shortcut, a calibrated path.
---

# Fast Track

**Core principle:** Small changes deserve proportionate process — not no process.

---

## Qualification Criteria

A change qualifies for fast track only if ALL of the following are true:

| Criterion | Check |
|---|---|
| ≤ 20 lines changed | Count before starting |
| Single file (or two tightly coupled files) | No system-wide ripple effects |
| No API contract changes | Nothing that callers depend on changes |
| No database or schema changes | State remains the same |
| No new dependencies introduced | Same library footprint |
| Reversible in one git revert | Clean, isolated |
| Intent is unambiguous | No design decision needed |

**If any criterion fails:** Use the full workflow (intention-mapping → task-graph → ...).

---

## The Fast Track Process

### 1. State the Change (2 sentences)

Before touching code, state explicitly:
- What is changing
- Why it's safe to change it without full workflow

```
Changing: Timeout value from 5000ms to 8000ms in src/api/client.ts
Safe because: Isolated config value, no callers depend on the specific value, easily reverted
```

### 2. Assumption Spot-Check (60 seconds)

Before acting, spot-check the top 2 assumptions:
- Is this file what you think it is?
- Are there any callers of the thing you're changing that you're not aware of?

Do a quick grep if needed. 60 seconds max.

### 3. Make the Change

Make the minimal change. If the change starts growing beyond the original 20-line scope: stop. Reclassify. Use full workflow.

### 4. Targeted Test

Run the test(s) directly related to the changed code. Not the full suite — but not nothing.

If no tests exist for this path: write one before the change. That's still fast track — it just includes a small test.

### 5. Verify Locally

Confirm the change has the intended effect. Don't ship based on reasoning alone.

---

## What Fast Track Is Not

- **Not a way to skip assumption validation** — you still do a spot-check
- **Not a carte blanche for "small" changes** — scope must actually be ≤ 20 lines
- **Not for decisions** — if you're unsure which approach to take, that's intention-mapping
- **Not cumulative** — three fast-track changes to the same file in one session = you should have used the full workflow

---

## Fast Track Accumulation Warning

If you're doing your 3rd fast-track change to the same area of code:
- Something about that area is attracting repeated changes
- That's a signal the original design needs revisiting, not more patches

Stop. Run a momentum-check on the area. Consider whether a proper refactor is needed.

---

## Documentation

Fast-track changes don't require a design doc or task-graph. They do require:
- A clear, descriptive commit message
- A note in the cognitive-offload backlog if there's any follow-up (e.g., "this timeout should be configurable — tracking")

---

## Red Flags

- "It's only a small change" without checking the criteria → Check the criteria. Always.
- Using fast-track for the 5th change this week to the same file → The file is telling you something. Listen.
- "I don't need to test this, it's obvious" → 60 seconds of targeted testing is proportionate. Do it.
- Fast track on anything touching auth, payments, or data integrity → These are not fast-track domains. Use the full workflow.
