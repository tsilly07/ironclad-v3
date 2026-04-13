---
name: systematic-debugging
description: Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes
---

# Systematic Debugging

**Core principle:** ALWAYS find root cause before attempting fixes. Symptom fixes are failure.

## The Iron Law

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

If you haven't completed Phase 1, you cannot propose fixes.

## When to Use

Use for ANY technical issue: test failures, bugs, unexpected behavior, performance problems, build/integration issues.

**Use ESPECIALLY when:** Under time pressure, "quick fix" seems obvious, you've tried multiple fixes, or you don't fully understand the issue.

## The Four Phases

### Phase 1: Root Cause Investigation

**BEFORE attempting ANY fix:**

1. **Read Error Messages Carefully** — Don't skip. Read stack traces completely. Note line numbers, file paths, error codes.

2. **Reproduce Consistently** — Can you trigger it reliably? Exact steps? If not reproducible → gather more data, don't guess.

3. **Check Recent Changes** — Git diff, recent commits, new dependencies, config changes, environmental differences.

4. **Gather Evidence in Multi-Component Systems** — Add diagnostic instrumentation at EACH component boundary. Log what enters/exits each component. Run once to find WHERE it breaks, THEN investigate that component. See `root-cause-tracing.md` for backward tracing technique.

5. **Trace Data Flow** — Where does bad value originate? Trace up the call stack until you find the source. Fix at source, not symptom.

### Phase 2: Pattern Analysis

1. **Find Working Examples** — Locate similar working code in same codebase.
2. **Compare Against References** — Read reference implementations COMPLETELY, not skimming.
3. **Identify Differences** — List every difference between working and broken, however small.
4. **Understand Dependencies** — Components, settings, config, environment, assumptions.

### Phase 3: Hypothesis and Testing

1. **Form Single Hypothesis** — "I think X is the root cause because Y." Be specific.
2. **Test Minimally** — SMALLEST possible change. One variable at a time.
3. **Verify** — Worked → Phase 4. Didn't work → NEW hypothesis. Don't stack fixes.

### Phase 4: Implementation

1. **Create Failing Test Case** — Use `ironclad:test-driven-development`. MUST have before fixing.
2. **Implement Single Fix** — ONE change. No "while I'm here" improvements.
3. **Verify Fix** — Test passes? No regressions? Issue resolved?
4. **If Fix Doesn't Work** — Count attempts. If <3: return to Phase 1. **If ≥3: STOP and question architecture.**
5. **If 3+ Fixes Failed** — Pattern indicates architectural problem (shared state/coupling, each fix reveals new problem). Discuss with your human partner before more attempts.

## Red Flags

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

**STOP and return to Phase 1 if you're thinking:**
- "Quick fix for now, investigate later"
- "Just try changing X and see"
- "I don't fully understand but this might work"
- "Here are the main problems: [fixes without investigation]"
- Proposing solutions before tracing data flow
- "One more fix attempt" (when already tried 2+)

## Human Partner Signals You're Doing It Wrong

- "Is that not happening?" → You assumed without verifying
- "Stop guessing" → You're proposing fixes without understanding
- "Ultrathink this" → Question fundamentals, not just symptoms

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Root Cause** | Read errors, reproduce, check changes, gather evidence | Understand WHAT and WHY |
| **2. Pattern** | Find working examples, compare | Identify differences |
| **3. Hypothesis** | Form theory, test minimally | Confirmed or new hypothesis |
| **4. Implementation** | Create test, fix, verify | Bug resolved, tests pass |

## Supporting Techniques

- **`root-cause-tracing.md`** — Trace bugs backward through call stack
- **`defense-in-depth.md`** — Add validation at multiple layers
- **`condition-based-waiting.md`** — Replace arbitrary timeouts with condition polling

**Related skills:** ironclad:test-driven-development, ironclad:verification-before-completion
