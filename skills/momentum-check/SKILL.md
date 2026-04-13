---
name: momentum-check
description: Use after any execution cycle completes — audits whether work is progressing or spinning, calculates a velocity score, and determines whether to continue, simplify, or re-plan.
---

# Momentum Check

**Core principle:** Effort is not progress. Measure velocity, not activity.

---

## When to Use

- After completing all tasks in a phase
- After any iteration on existing work
- When something feels "almost done" for more than one session
- When the human partner asks "how's it going?"

---

## The Velocity Score

Calculate a velocity score after each execution cycle:

```
Velocity = (Tasks Closed) / (Tasks Closed + Tasks Opened)
```

Where:
- **Tasks Closed** = tasks completed and passed ship-gate
- **Tasks Opened** = new tasks discovered or created during execution

| Score | Interpretation | Action |
|---|---|---|
| 0.8 – 1.0 | Excellent momentum | Continue current approach |
| 0.6 – 0.79 | Good progress, mild scope growth | Continue, monitor |
| 0.4 – 0.59 | Scope growth equals progress | Pause. Scope containment check. |
| 0.2 – 0.39 | Scope growing faster than progress | Stop. Re-plan required. |
| 0.0 – 0.19 | No net progress | Full reassessment. |

---

## Step 1 — Count Accurately

Build an honest ledger:

```markdown
## Cycle Ledger — [Feature/Iteration Name]
Period: [phase or session]

### Tasks Closed (✅ ship-gate passed)
- T-01: User auth endpoint
- T-03: Token refresh logic
- T-06: Error middleware

### Tasks Opened During This Cycle
- T-08: Rate limiting on auth endpoint (discovered: Dimension 3 of adversarial review)
- T-09: Session log schema needed (discovered: T-03 implementation)

### Velocity
Tasks Closed: 3
Tasks Opened: 2
Velocity: 3 / (3 + 2) = 0.60 ✅ (Good)
```

---

## Step 2 — Quality Audit

Velocity tells you speed. Quality audit tells you direction.

For each closed task, ask:
- Is the ship-gate sign-off complete and honest?
- Are there any known issues deferred without a tracked item?
- Did this task produce any new assumptions that haven't been validated?

If any closed task would fail a quality audit: it is not truly closed. Reopen it.

---

## Step 3 — Complexity Trend

Did the system get more complex or less complex this cycle?

```markdown
## Complexity Trend
- New abstractions introduced: 2
- Abstractions removed/simplified: 0
- Net complexity change: +2

Assessment: Complexity is growing. Next cycle should include simplification.
```

A system that always grows in complexity becomes unmaintainable. Track the trend.

---

## Step 4 — Next Cycle Recommendation

Based on velocity + quality audit + complexity trend:

| Condition | Recommendation |
|---|---|
| Velocity ≥ 0.8, quality OK, complexity stable | ✅ Continue to next phase |
| Velocity 0.6–0.79, quality OK | ✅ Continue, add scope-containment check to next cycle |
| Velocity < 0.6 or quality issues | ⚠️ Simplification pass before next features |
| Velocity < 0.4 | 🛑 Re-plan required. Do not add features. |
| Quality audit fails on any closed task | 🛑 Reopen and fix before continuing |

---

## Simplification Pass

When complexity is growing or velocity is declining, a simplification pass may be necessary:

A simplification pass is a dedicated cycle with zero new features:
- Reduce: remove code that isn't used
- Clarify: rename, restructure for readability
- Consolidate: merge duplicated logic
- Document: capture things that exist only in your head

Simplification passes have their own task-graph and their own ship-gates. They are not an excuse to quietly refactor everything.

---

## The "Almost Done" Trap

If a task or feature has been "almost done" for more than one session:
1. Run a momentum check on that specific item
2. Count how many tasks opened vs closed in that item's name
3. The answer will usually explain the almost-done state

"Almost done" is not a project state. It's a signal that scope is unconstrained.

---

## Red Flags

- "We're making great progress" without numbers → Calculate the velocity score.
- "Just a few more things" → Count them. Add them to Tasks Opened.
- "This is more complex than we thought" → Complexity trend analysis. Is this growing out of control?
- "We'll clean this up later" → Add it to Tasks Opened. It counts against velocity.
