---
name: requesting-code-review
description: Use when completing tasks, implementing major features, or before merging to verify work meets requirements
---

# Requesting Code Review

Dispatch ironclad:code-reviewer subagent to catch issues before they cascade. The reviewer gets precisely crafted context — never your session's history.

**Core principle:** Review early, review often.

## When to Request

**Mandatory:** After each task in subagent-driven development, after completing major feature, before merge to main.

**Optional:** When stuck (fresh perspective), before refactoring, after fixing complex bug.

## How to Request

1. **Get git SHAs:**
```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

2. **Dispatch code-reviewer subagent** using Task tool with template at `code-reviewer.md`. Fill placeholders: `{WHAT_WAS_IMPLEMENTED}`, `{PLAN_OR_REQUIREMENTS}`, `{BASE_SHA}`, `{HEAD_SHA}`, `{DESCRIPTION}`.

3. **Act on feedback:**
   - **Critical:** Fix immediately
   - **Important:** Fix before proceeding
   - **Minor:** Note for later
   - **Wrong:** Push back with reasoning

## Integration

**Subagent-Driven Development:** Review after EACH task.
**Executing Plans:** Review after each batch.
**Ad-Hoc:** Review before merge or when stuck.

## Red Flags

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

**Never:** Skip review because "it's simple", ignore Critical issues, proceed with unfixed Important issues.

See template at: requesting-code-review/code-reviewer.md
