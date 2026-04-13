---
name: using-git-worktrees
description: Use when starting feature work that needs isolation from current workspace or before executing implementation plans - creates isolated git worktrees with smart directory selection and safety verification
---

# Using Git Worktrees

Git worktrees create isolated workspaces sharing the same repository.

**Core principle:** Systematic directory selection + safety verification = reliable isolation.

**Announce at start:** "I'm using the using-git-worktrees skill to set up an isolated workspace."

## Directory Selection (Priority Order)

1. **Check existing:** `ls -d .worktrees worktrees 2>/dev/null` — if both exist, `.worktrees` wins.
2. **Check CLAUDE.md:** `grep -i "worktree.*director" CLAUDE.md` — use preference without asking.
3. **Ask user:** Offer `.worktrees/` (project-local, hidden) or `~/.config/ironclad/worktrees/<project>/` (global).

## Safety Verification

**Project-local directories:** MUST verify ignored before creating:
```bash
git check-ignore -q .worktrees 2>/dev/null
```
If NOT ignored: add to .gitignore, commit, then proceed.

**Global directory (~/.config/ironclad/worktrees):** No verification needed.

## Creation Steps

1. **Detect project:** `project=$(basename "$(git rev-parse --show-toplevel)")`
2. **Create worktree:** `git worktree add "$path" -b "$BRANCH_NAME"` → `cd "$path"`
3. **Run setup:** Auto-detect from project files (package.json → npm install, Cargo.toml → cargo build, requirements.txt → pip install, go.mod → go mod download)
4. **Verify baseline:** Run test suite. If tests fail → report, ask whether to proceed. If pass → report ready.
5. **Report:**
```
Worktree ready at <full-path>
Tests passing (<N> tests, 0 failures)
Ready to implement <feature-name>
```

## Quick Reference

| Situation | Action |
|-----------|--------|
| `.worktrees/` exists | Use it (verify ignored) |
| `worktrees/` exists | Use it (verify ignored) |
| Both exist | Use `.worktrees/` |
| Neither exists | Check CLAUDE.md → Ask user |
| Directory not ignored | Add to .gitignore + commit |
| Tests fail during baseline | Report + ask |

## Red Flags

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

**Never:** Create worktree without verifying it's ignored (project-local), skip baseline test verification, proceed with failing tests without asking.

## Integration

**Called by:** brainstorming, subagent-driven-development, executing-plans.
**Pairs with:** finishing-a-development-branch (cleanup).
