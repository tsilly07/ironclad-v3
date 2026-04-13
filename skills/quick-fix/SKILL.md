---
name: quick-fix
description: Use for trivial changes that don't justify the full brainstorm → plan → execute workflow — config edits, typos, single-line fixes, value changes, simple additions under 20 lines
---

# Quick Fix

**Core principle:** Not every change needs a ceremony. Discipline means knowing WHEN to apply process, not applying it blindly to everything.

## When to Use

The full Ironclad workflow (brainstorm → plan → TDD → review) exists for good reason — but it's overkill for:

- Changing a config value (price, spawn point, timer)
- Fixing a typo or string
- Adding an item to an existing list/table
- Enabling/disabling a feature flag
- Simple copy-paste of an existing pattern with different values
- Changes under 20 lines total

## When NOT to Use

**Go back to full workflow if:**
- Change touches logic (if/else, loops, new functions)
- Change affects multiple files
- Change could break other things
- You're not 100% sure what the change does
- Human partner asked for a "feature" not a "fix"

**When in doubt: use the full workflow.** Quick Fix is for obvious, safe changes only.

## The Process

1. **State the change** in one sentence: "Changing water price from 50 to 75 in shop config"
2. **Make the change** — no planning, no brainstorm, just do it
3. **Verify** — does it look right? Any obvious issues?
4. **Commit** — `git commit -m "fix: adjust water price to 75"`
5. **Tell human** — "Done. Changed X to Y in Z file."

Total time: 1-2 minutes. Total tokens: ~5K.

## Quick Fix Checklist

Before using Quick Fix, all must be true:

- [ ] Change is under 20 lines
- [ ] No new logic (no if/else, no new functions)
- [ ] Single file affected
- [ ] Obvious what the change does
- [ ] Low risk of breaking other things

**Any checkbox false? → Full workflow.**

## Batch Quick Fixes

If human partner gives a list of simple changes:

```
"Change these prices: water=75, bread=50, medkit=200"
```

Do all of them in one pass, one commit:
```bash
git commit -m "fix: adjust shop prices (water=75, bread=50, medkit=200)"
```

Don't run brainstorm → plan → TDD for each price change individually.

## Integration with Token Budget

Quick Fix is Minimal tier by design. If you're spending more than ~10K tokens on a Quick Fix, it's not a Quick Fix anymore — switch to Standard tier and use the full workflow.

## Red Flags

- "Let me brainstorm the best approach for changing this number" → It's a number. Change it.
- "Writing a test for this config value change" → Config values don't need unit tests.
- "Let me plan out the implementation" → There's nothing to plan. Just edit the file.
- BUT: "This simple change actually touches 3 files" → Not a Quick Fix. Full workflow.
