---
name: finishing-a-development-branch
description: Use when implementation is complete, all tests pass, and you need to decide how to integrate the work - guides completion of development work by presenting structured options for merge, PR, or cleanup
---

# Finishing a Development Branch

**Core principle:** Verify tests → Present options → Execute choice → Clean up.

**Announce at start:** "I'm using the finishing-a-development-branch skill to complete this work."

## Step 1: Verify Tests

Run project's test suite. **If tests fail:** Report failures, stop. Don't proceed. **If tests pass:** Continue.

## Step 2: Determine Base Branch

```bash
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

Or ask: "This branch split from main - is that correct?"

## Step 3: Present Options

Present exactly these 4 options:

```
Implementation complete. What would you like to do?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

## Step 4: Execute Choice

**Option 1 — Merge Locally:** Checkout base → pull latest → merge feature → verify tests on result → delete feature branch → cleanup worktree.

**Option 2 — Push and Create PR:** Push branch → `gh pr create` with summary + test plan → cleanup worktree.

**Option 3 — Keep As-Is:** Report branch and worktree location. Don't cleanup.

**Option 4 — Discard:** Confirm first (list branch, commits, worktree). Require typed "discard" confirmation. Then delete branch and cleanup worktree.

## Step 5: Cleanup Worktree

For Options 1, 2, 4: `git worktree remove <path>`. For Option 3: keep worktree.

## Quick Reference

| Option | Merge | Push | Keep Worktree | Cleanup Branch |
|--------|-------|------|---------------|----------------|
| 1. Merge | ✓ | - | - | ✓ |
| 2. PR | - | ✓ | ✓ | - |
| 3. Keep | - | - | ✓ | - |
| 4. Discard | - | - | - | ✓ (force) |

## Red Flags

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

**Never:** Proceed with failing tests, merge without verifying tests on result, delete work without confirmation, force-push without explicit request.

## Integration

**Called by:** subagent-driven-development, executing-plans.
**Pairs with:** using-git-worktrees (cleans up worktree created by that skill).
