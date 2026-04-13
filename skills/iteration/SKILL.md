---
name: iteration
description: Use after completing an execution cycle (brainstorm → plan → execute) when changes are needed to an already-implemented feature — missed edge cases, additional features, or design adjustments
---

# Iteration

Structured workflow for refining work after the initial implementation cycle. Prevents falling back to ad-hoc "vibe coding" or wastefully re-running the full brainstorm cycle.

**Core principle:** The initial implementation is rarely final. Iteration needs the same discipline as the first pass, but without redundant re-discovery of context that already exists.

## When to Use

- You completed a brainstorm → plan → execute cycle
- Changes are needed (missed edge case, new requirement, design adjustment)
- The core design is still sound — only incremental changes needed

**Don't use if:** The core design is fundamentally wrong → re-brainstorm instead.

## The Process

### 1. Delta Assessment

Load the existing design doc and compare against current state:

- What changed since the spec was written?
- What new requirements emerged?
- What broke or doesn't work as expected?

Classify the iteration:

| Type | Scope | Action |
|------|-------|--------|
| **Patch** | Bug fix, edge case, typo | Skip to targeted plan |
| **Extension** | New feature on existing design | Quick design delta → targeted plan |
| **Revision** | Design adjustment that affects multiple components | Design delta with impact analysis → targeted plan |

### 2. Design Delta (Extension/Revision only)

Write a short delta document — NOT a full redesign:

```markdown
# Iteration: [topic]
## Parent spec: [path to original spec]
## Changes needed:
- [specific change 1]
- [specific change 2]
## Impact on existing implementation:
- [what existing code/tests are affected]
## New components (if any):
- [new things to build]
```

Save to `docs/ironclad/specs/YYYY-MM-DD-<topic>-iteration-N.md`

### 3. Targeted Plan

Create a plan covering ONLY the changes. Reference the original plan for context but don't re-plan completed work.

Each task must specify:
- What existing code/tests to modify
- What new code/tests to add
- How to verify the change doesn't break existing functionality

### 4. Execute

Use subagent-driven-development or executing-plans — same quality gates as initial implementation. TDD still applies. Reviews still apply.

### 5. Verify Integration

After all iteration tasks complete:
- Run full test suite (not just new tests)
- Verify original functionality still works
- Check that the iteration didn't introduce inconsistencies with the original spec

## Auto-Approve Gates

For Patch iterations with high confidence:
- Diff <20 lines
- All existing tests still pass
- New test covers the fix
- Single file changed

→ Agent may proceed without human checkpoint.

For Extension/Revision: always checkpoint with human after the design delta.

## Iteration on Iteration

If you're iterating on an iteration, that's a signal to step back:
- Are you accumulating patches? Consider a clean refactor pass.
- Are you on iteration 3+? The original design may need revisiting.

## Red Flags

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

- "I'll just make a quick fix" → Follow the iteration process. Quick fixes become tech debt.
- "The tests still pass so it's fine" → Passing tests ≠ correct behavior. Verify against spec.
- "This is too small for a plan" → Even Patch iterations get a plan. It can be 3 lines.
