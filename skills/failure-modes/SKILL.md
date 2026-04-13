---
name: failure-modes
description: Use before starting any non-trivial task — proactively maps every way this implementation can fail, sets tripwires, and defines pre-planned recovery paths. Prevention is cheaper than recovery.
---

# Failure Modes

**Core principle:** Every failure was predictable. Map failure modes before they happen, not after.

---

## This Is Not Error Recovery

`failure-modes` is pre-execution. `error-recovery` is reactive. This skill runs *before* you start, so that when problems arise, you already have a plan — not a scramble.

---

## When to Use

- Before starting any High/Extreme complexity task (from task-graph)
- Before executing any High Risk task
- When a task has external dependencies (APIs, databases, third-party systems)
- Before deploying or running irreversible operations

---

## Step 1 — Enumerate Failure Modes

For the task at hand, list every realistic way it can fail. Organize by category:

### Category A — Technical Failures
Things that break in the code or environment:
- Syntax / type errors
- Logic errors (edge cases, off-by-one)
- Missing dependencies / version mismatches
- Race conditions / timing issues
- Memory / resource exhaustion

### Category B — Integration Failures
Things that break at boundaries:
- API contract violations (expected vs actual response shape)
- Auth / permission failures
- Network timeouts / unavailable services
- Data format mismatches between systems

### Category C — State Failures
Things that break because the world wasn't in the expected state:
- Database in unexpected state
- File not found / wrong path
- Config missing or malformed
- External service changed behavior

### Category D — Requirements Failures
Things that break because you built the wrong thing:
- Misunderstood requirement
- Assumption invalidated mid-execution
- Scope drift from original intent
- Performance doesn't meet threshold

---

## Step 2 — Rate Each Failure Mode

| Failure Mode | Likelihood (0–100%) | Impact (Low/Med/High/Critical) | Detectability (Easy/Hard) |
|---|---|---|---|
| API returns 500 on edge input | 40% | High | Easy |
| Race condition on concurrent writes | 20% | Critical | Hard |
| Wrong assumption on auth flow | 60% | High | Easy |

Priority = Likelihood × Impact. Address Critical+High likelihood modes first.

---

## Step 3 — Set Tripwires

For each High or Critical failure mode, define a **tripwire** — a detectable signal that the failure mode is occurring:

```markdown
### Failure Mode: Race condition on concurrent writes
- Tripwire: Test with 50 concurrent requests. Look for non-deterministic output.
- Detection point: Before merging. In CI, not in dev.
- Owner: Task T-04
```

Tripwires should trigger *before* the failure causes damage, not after.

---

## Step 4 — Pre-Plan Recovery Paths

For each Critical failure mode, write the recovery path *now*, before you need it:

```markdown
### Recovery: API contract violation
If the external API returns an unexpected shape:
1. Log the full raw response
2. Return a safe default (never crash the caller)
3. Alert monitoring
4. Do NOT auto-retry more than 3 times (backoff)
5. Escalate to human if retry limit hit
```

This is different from `root-cause-isolation` — that's for after the failure. This is the *plan* you'll execute when you hit the tripwire.

---

## Step 5 — Failure Mode Log

Save to `docs/ironclad/risks/YYYY-MM-DD-<task>-failures.md`:

```markdown
## Failure Mode Log — [Task Name]

| ID | Failure Mode | Category | Likelihood | Impact | Tripwire | Recovery Path |
|---|---|---|---|---|---|---|
| F-01 | Auth token expired mid-session | Integration | 70% | High | 401 response caught in middleware | Refresh token, retry once |
| F-02 | DB migration ran on wrong env | State | 15% | Critical | Pre-flight env check | Restore from backup |
```

---

## Failure Mode Debt

If you don't have time to plan recovery for all failures, explicitly flag the ones you're accepting as debt:

```markdown
### Accepted Risk (Debt)
- F-03: Concurrent write race — not addressing in this task. Tracking in issue #47.
```

Never silently skip failure modes. Make the risk explicit so the human can decide.

---

## Red Flags

- "This is unlikely to fail" → Rate it. If it's 5%, it's acceptable. If it's 40%, it's not.
- "I'll handle errors as they come up" → This is unplanned. Pre-planning takes 10 minutes and saves hours.
- "The framework handles this" → That's an assumption. Rate its confidence.
- "We can fix it in production" → Production failures cost 10x more. Map it now.
