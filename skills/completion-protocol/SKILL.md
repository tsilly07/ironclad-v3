---
name: completion-protocol
description: Use when a development branch is ready to close — produces a structured handoff artifact including diff summary, test evidence, migration plan, rollback instructions, and a formal decision on branch fate.
---

# Completion Protocol

**Core principle:** Finishing is not merging. Finishing is handoff with full evidence.

---

## When to Use

- When all tasks in a feature branch are complete and ship-gate signed off
- When wrapping up a worktree or long-lived branch
- When handing work to another human or agent
- When closing out an experiment (including the decision to discard it)

---

## Step 1 — Pre-Completion Checklist

Before producing the handoff artifact:

- [ ] All tasks in the task-graph show ✅ (ship-gate passed)
- [ ] Full test suite passes on the branch (not just per-task tests)
- [ ] adversarial-review completed for all High Complexity tasks
- [ ] All 🔴 Critical review findings resolved
- [ ] All scope expansions approved and incorporated into task-graph
- [ ] momentum-check shows Velocity ≥ 0.6
- [ ] No open assumptions with <80% confidence

If any item fails: the branch is not ready for completion protocol. Go fix it.

---

## Step 2 — Diff Summary

Produce a human-readable summary of what changed:

```markdown
## Changes Summary — [Feature Name]

### New Capabilities
- Users can now reset passwords via email link
- Rate limiting applied to all auth endpoints (10 req/min)

### Files Changed
| File | Type | Description |
|---|---|---|
| `src/auth/reset.ts` | New | Password reset flow |
| `src/middleware/rateLimit.ts` | New | Rate limiting middleware |
| `src/routes/auth.ts` | Modified | Added reset route, applied rate limiter |
| `tests/auth/reset.test.ts` | New | Full test suite for reset flow |

### Stats
- Files changed: 4
- Lines added: 287
- Lines removed: 14
- Test coverage: 12 new tests, all passing
```

---

## Step 3 — Migration Plan (if applicable)

If this branch includes database migrations, config changes, or environment variable changes:

```markdown
## Migration Plan

### Database
- Migration file: `migrations/2025-01-15-add-password-reset-tokens.sql`
- Reversible: YES (see rollback below)
- Zero-downtime: YES (additive only — new table)
- Estimated time: <1 second

### Config / Environment
- New env var: `RESET_TOKEN_EXPIRY_HOURS` (default: 24)
- Required before deploy: NO (has default)

### Data
- No existing data affected
```

---

## Step 4 — Rollback Instructions

Detailed, copy-paste-ready rollback instructions:

```markdown
## Rollback Instructions

### If reverting before deploy
git revert <merge-commit-sha> --mainline 1
git push origin main

### If reverting after deploy (application live)
1. git revert <merge-commit-sha> --mainline 1 && git push
2. Run: psql $DATABASE_URL -f migrations/rollback-2025-01-15.sql
3. Remove RESET_TOKEN_EXPIRY_HOURS from env
4. Redeploy

### Estimated rollback time: 10–15 minutes
### Data impact: None (rollback drops empty table only)
```

---

## Step 5 — Branch Fate Decision

Choose one:

| Decision | When | Action |
|---|---|---|
| **Merge to main** | Work is production-ready | Create PR or merge directly |
| **Merge behind flag** | Code ready, feature not yet | Merge with feature flag OFF |
| **Keep for later** | Work is good, not prioritized | Archive branch, document state |
| **Discard** | Experiment failed or approach changed | Delete branch, document learnings |

For **Discard**: write a learnings note — what was tried, what didn't work, why discarded. This is valuable even though the code is gone.

---

## Step 6 — Handoff Artifact

Save to `docs/ironclad/handoffs/YYYY-MM-DD-<feature>-completion.md` (full aggregation of steps 2–5).

If handing to another agent: include the cognitive-offload `index.md` state at time of handoff.

If handing to a human: include the PR link and a 3-sentence summary.

---

## PR Template

If creating a PR:

```markdown
## What This Does
[2-3 sentences max]

## Evidence
- [ ] All tests pass (link to CI)
- [ ] adversarial-review: [link]
- [ ] ship-gate signed off: [link]

## Rollback
[Copy rollback instructions from Step 4]

## Migration Notes
[Or: "No migrations required"]
```

---

## Red Flags

- Merging without a diff summary → the reviewer is flying blind
- "Rollback = just revert the commit" → test that assumption before stating it
- Discarding a branch with no learnings doc → institutional knowledge lost
- "The tests are in the PR" → Evidence should be in the handoff artifact, not assumed from context
