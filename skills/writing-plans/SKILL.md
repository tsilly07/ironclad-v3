---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

Write comprehensive implementation plans assuming the engineer has zero context and questionable taste. Document everything: files, code, testing, verification. Bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** Run in a dedicated worktree (created by brainstorming skill).

**Save to:** `docs/ironclad/plans/YYYY-MM-DD-<feature-name>.md` (user preferences override).

## Scope Check

If spec covers multiple independent subsystems, suggest breaking into separate plans — one per subsystem. Each plan should produce working, testable software on its own.

## File Structure

Map out files before defining tasks. Design units with clear boundaries and one responsibility each. Prefer smaller, focused files. Follow existing patterns in the codebase.

## Task Granularity

**Each step is one action (2-5 minutes):** Write failing test → Run to verify it fails → Implement minimal code → Run to verify it passes → Commit.

## Plan Document Header

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use ironclad:subagent-driven-development (recommended) or ironclad:executing-plans

**Goal:** [One sentence]
**Architecture:** [2-3 sentences]
**Tech Stack:** [Key technologies]
---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**
[actual test code]

- [ ] **Step 2: Run test to verify it fails**
Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**
[actual implementation code]

- [ ] **Step 4: Run test to verify it passes**
Expected: PASS

- [ ] **Step 5: Commit**
```bash
git add <files>
git commit -m "feat: add specific feature"
```
````

## No Placeholders

Every step must contain actual content. **Never write:** "TBD", "TODO", "implement later", "add appropriate error handling", "similar to Task N" (repeat the code), steps describing what to do without code blocks, references to undefined types/functions.

## Self-Review

After writing the plan, check against spec:
1. **Spec coverage:** Can you point to a task for each requirement? List gaps.
2. **Placeholder scan:** Any patterns from "No Placeholders" above? Fix them.
3. **Type consistency:** Do names/signatures match across tasks?

Fix inline. No re-review needed.

## Execution Handoff

After saving, offer:
1. **Subagent-Driven (recommended)** → ironclad:subagent-driven-development
2. **Inline Execution** → ironclad:executing-plans
