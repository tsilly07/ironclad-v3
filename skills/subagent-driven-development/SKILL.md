---
name: subagent-driven-development
description: Use when executing implementation plans with independent tasks in the current session
---

# Subagent-Driven Development

Execute plan by dispatching fresh subagent per task, with adaptive review based on task complexity.

**Why subagents:** Isolated context per task. You construct exactly what they need — they never inherit your session's context. This preserves your context for coordination.

**Core principle:** Fresh subagent per task + adaptive review = high quality, fast iteration.

## When to Use

- Have an implementation plan with mostly independent tasks
- Want to stay in the current session (vs. executing-plans for parallel sessions)

## Adaptive Review

Not all tasks need the same review depth. Classify before dispatching:

### Trivial (single-pass review)
Criteria: diff <20 lines, config/rename/formatting changes, single file touched.
→ Spec reviewer only. Skip code quality review.

### Standard (spec review + spot check)
Criteria: 20-100 line diff, clear spec, 1-3 files.
→ Spec compliance review. Code quality review only if spec reviewer flags concerns.

### Complex (full two-stage review)
Criteria: >100 line diff, multi-file integration, security-sensitive, new APIs, architectural changes.
→ Full spec compliance review, then full code quality review. Both mandatory.

**Default to Standard if uncertain.** Upgrade to Complex if any doubt about correctness.

## The Process

1. Read plan, extract all tasks with full text, note context, create TodoWrite
2. For each task:
   a. Classify complexity (Trivial/Standard/Complex)
   b. Dispatch implementer subagent with full task text + context
   c. Handle implementer status (DONE/DONE_WITH_CONCERNS/NEEDS_CONTEXT/BLOCKED)
   d. Run review per complexity classification
   e. Fix loop if reviewer finds issues → implementer fixes → re-review
   f. Mark task complete
3. After all tasks: dispatch final code reviewer for entire implementation
4. Invoke finishing-a-development-branch

## Model Selection

Use the least powerful model that handles each role:

- **Mechanical tasks** (isolated functions, clear specs, 1-2 files) → cheap model
- **Integration tasks** (multi-file coordination, debugging) → standard model
- **Architecture/design/review** → most capable model

## Handling Implementer Status

**DONE:** Proceed to review.
**DONE_WITH_CONCERNS:** Read concerns. If correctness/scope → address before review. If observations → note and proceed.
**NEEDS_CONTEXT:** Provide missing context, re-dispatch.
**BLOCKED:** Assess: context problem → provide more context. Task too hard → more capable model. Task too large → break down. Plan wrong → escalate to human. **Never** retry without changes.

## Prompt Templates

- `./implementer-prompt.md`
- `./spec-reviewer-prompt.md`
- `./code-quality-reviewer-prompt.md`

## Red Flags

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

**Never:**
- Start implementation on main/master without explicit user consent
- Skip reviews or proceed with unfixed issues
- Dispatch multiple implementation subagents in parallel
- Make subagent read plan file (provide full text instead)
- Skip scene-setting context
- Accept "close enough" on spec compliance
- Start code quality review before spec compliance passes
- Move to next task with open review issues

## Integration

**Required:** using-git-worktrees, writing-plans, requesting-code-review, finishing-a-development-branch
**Subagents use:** test-driven-development
**Alternative:** executing-plans (parallel session)
