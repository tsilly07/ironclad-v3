---
name: branch-strategy
description: Use before starting any non-trivial implementation — defines full branch strategy including risk assessment, merge windows, feature flag consideration, and long-lived branch protocols. Not just isolation — strategy.
---

# Branch Strategy

**Core principle:** A branch without a strategy is a liability. Know how it merges before you create it.

---

## This Is Not Just "Create a Branch"

Creating a git worktree for isolation is table stakes. Branch strategy asks:
- What is the risk profile of this branch?
- When and how does it merge?
- What's the rollback plan if it can't merge?
- Does this need a feature flag?
- How long will it live, and what's the plan if it lives longer?

---

## Step 1 — Branch Risk Assessment

Before creating the branch, classify its risk:

| Dimension | Questions |
|---|---|
| **Scope** | How many files will this touch? How many systems? |
| **Reversibility** | Can the changes be reverted cleanly after merge? |
| **Coupling** | Does this branch depend on other in-flight branches? |
| **Blast radius** | If something goes wrong after merge, what breaks? |
| **Duration** | How long is this expected to live? |

Score each dimension: Low / Medium / High

**Overall Risk:**
- All Low → Low risk branch
- Any High → High risk branch
- Mixed → Medium risk branch

---

## Step 2 — Choose Branch Type

| Type | When | Lifetime | Merge Strategy |
|---|---|---|---|
| **Feature branch** | Standard new feature | 1–5 days | Squash or merge commit |
| **Long-lived feature** | Complex feature, >1 week | 1–4 weeks | Merge commit, frequent rebases |
| **Hotfix** | Production bug fix | Hours | Fast-forward if possible |
| **Experiment** | Proof of concept | Days to weeks | May never merge — explicit decision point |
| **Integration** | Combines multiple feature branches | Tied to feature lifecycle | Merge after all sub-branches merge |

---

## Step 3 — Feature Flag Decision

Should this feature be hidden behind a flag during merge?

**Use a feature flag when:**
- The feature is complete but not ready for all users
- You want to merge early and often into main (recommended for long-lived branches)
- You need a kill switch in production

**Don't use a flag when:**
- The change is structural or invisible to users
- The branch is short-lived (<3 days)
- The flag itself would add complexity that outweighs the benefit

---

## Step 4 — Merge Window

Define when this branch can and cannot merge:

```markdown
## Merge Window — [Branch Name]
Earliest: After T-05 and T-06 complete
Latest: Before the deployment scheduled for [date]
Blocked by: [other branch] must merge first
Cannot merge during: [high-traffic periods, other team deploys]
```

---

## Step 5 — Long-Lived Branch Protocol

If the branch is expected to live more than 1 week:

**Daily:**
- Rebase onto main (or merge main in) at start of each session
- Run tests after every rebase

**Weekly:**
- Review the branch diff: is it still reasonable?
- Check for conflicts with other in-flight branches
- Re-assess merge window

**At 2 weeks:**
- Force a branch review decision:
  - Merge now (even if incomplete, behind a flag)?
  - Continue (with updated timeline)?
  - Archive (experiment failed or deprioritized)?

A branch that lives indefinitely is a liability.

---

## Step 6 — Rollback Plan

Before any merge:

```markdown
## Rollback Plan — [Branch Name]

Post-merge revert:
- Command: `git revert <merge-commit-sha> --mainline 1`
- Revert takes: ~10 minutes
- Side effects of revert: [list any db migrations that would need reverting too]
- Revert tested in staging: YES / NO / N/A
```

If reverting is complex (e.g., database migrations), document the full procedure.

---

## Red Flags

- "I'll figure out the merge strategy later" → You're creating a liability. Decide now.
- Branch has been alive for 3 weeks with no merge decision → Run the long-lived protocol.
- "We don't need a rollback plan" → Then you don't understand the blast radius.
- Two feature branches both touch the same file → Coupling risk. Coordinate or serialize.
