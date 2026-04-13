---
name: verification-before-completion
description: Use when about to claim work is complete, fixed, or passing, before committing or creating PRs - requires running verification commands and confirming output before making any success claims; evidence before assertions always
---

# Verification Before Completion

**Core principle:** Evidence before claims, always.

## The Iron Law

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

If you haven't run the verification command in this message, you cannot claim it passes.

## The Gate Function

1. **IDENTIFY:** What command proves this claim?
2. **RUN:** Execute the FULL command (fresh, complete)
3. **READ:** Full output, check exit code, count failures
4. **VERIFY:** Does output confirm the claim?
5. **ONLY THEN:** Make the claim

Skip any step = lying, not verifying.

## What Each Claim Requires

| Claim | Requires | Not Sufficient |
|-------|----------|----------------|
| Tests pass | Test output: 0 failures | Previous run, "should pass" |
| Linter clean | Linter output: 0 errors | Partial check |
| Build succeeds | Build command: exit 0 | Linter passing |
| Bug fixed | Original symptom: passes | "Code changed" |
| Agent completed | VCS diff shows changes | Agent reports "success" |
| Requirements met | Line-by-line checklist | Tests passing |

## Red Flags — STOP

- Using "should", "probably", "seems to"
- Expressing satisfaction before verification ("Great!", "Done!")
- About to commit/push/PR without verification
- Trusting agent success reports
- Relying on partial verification
- ANY wording implying success without running verification

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

## Key Patterns

**Tests:** Run → see "N/N pass" → THEN claim.
**Regression (TDD):** Write → Run (pass) → Revert fix → Run (MUST FAIL) → Restore → Run (pass).
**Build:** Run build → see exit 0 → THEN claim.
**Requirements:** Re-read plan → checklist → verify each → report.
**Agent delegation:** Agent reports → check VCS diff → verify changes → report actual state.

## When to Apply

**ALWAYS before:** Any success/completion claim, any positive statement about work state, committing, PR creation, task completion, moving to next task.

This is non-negotiable.
