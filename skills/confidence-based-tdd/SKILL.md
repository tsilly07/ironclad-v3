---
name: confidence-based-tdd
description: Use during all implementation work — extends RED-GREEN-REFACTOR with confidence scoring, behavioral correctness checks, and chaos scenarios. Tests that pass are not enough. Tests must prove behavior.
---

# Confidence-Based TDD

**Core principle:** Passing tests are necessary but not sufficient. A test that passes for the wrong reason is worse than no test.

---

## Beyond RED-GREEN-REFACTOR

Standard TDD says: write failing test, make it pass, refactor. This is correct but incomplete.

Confidence-based TDD adds:
1. **Why** does the test fail? (not just that it does)
2. **What** exactly does the test prove? (not just that it passes)
3. **How confident** are we that the test covers the real behavior?
4. **What would break this** if the implementation were subtly wrong?

---

## The Full Cycle

### Phase 1 — RED (Write the test that fails)

Before writing the test, answer:
- What behavior am I proving?
- What is the precise failing condition?
- If I wrote this test and it *passed* immediately, what would that tell me?

```
Acceptable RED: Test fails because the feature doesn't exist yet
Unacceptable RED: Test fails because of a typo in the test
Suspicious RED: Test fails for a reason you don't fully understand
```

**Suspicious RED rule:** If a test fails and you're not 100% sure why, stop. Understand the failure before proceeding. An unexplained failure means you're not in control.

---

### Phase 2 — CONFIDENCE SCORE (Before going GREEN)

Rate your test before making it pass:

| Dimension | Question | Score (0–100%) |
|---|---|---|
| **Specificity** | Does this test fail for exactly one reason? | |
| **Coverage** | Does one passing implementation guarantee correct behavior? | |
| **Isolation** | Is this test unaffected by other tests or global state? | |
| **Readability** | Will the next person understand what this tests and why? | |
| **Fragility** | Will this break from unrelated changes? (low = good) | |

**If Specificity < 80% or Coverage < 70%**: rewrite the test before going GREEN.

A test with low confidence is worse than no test — it gives false assurance.

---

### Phase 3 — GREEN (Minimal implementation)

Write the minimum code to make the test pass. Not the best code. The minimal code.

**Anti-patterns:**
- Writing more code than needed to "be safe" → this hides coverage gaps
- Making the test pass by hardcoding the expected output → test covers no behavior
- Changing the test to make it easier to pass → never acceptable

If you can't make the test pass without also writing extra code: the test is too broad. Narrow it.

---

### Phase 4 — BEHAVIOR VERIFICATION

Before moving to REFACTOR, verify behavioral correctness — not just test passage.

Run the mental model:
```
"If my implementation were subtly wrong in [specific way], would this test catch it?"
```

For each test, identify at least one mutation to the implementation that *should* break the test:

```markdown
Test: `add(2, 3)` returns `5`
Mutation: Change `+` to `-` → test fails ✅ (test is valid)

Test: `getUser(id)` returns a user object
Mutation: Return a different user's data → test passes ❌ (test is invalid — it only checks shape, not identity)
```

If you can't find a mutation that breaks the test: the test proves nothing. Rewrite it.

---

### Phase 5 — REFACTOR

Improve the implementation without changing behavior. Tests are your safety net.

Rules:
- All tests must still pass after refactor
- Refactor one thing at a time
- Commit before and after each significant refactor
- If a refactor causes test failures: revert, understand, then refactor differently

---

### Phase 6 — CHAOS SCENARIOS (for High Complexity tasks)

After the main test suite passes, add at least 2 chaos scenarios:

| Scenario Type | Example |
|---|---|
| Concurrent calls | 100 simultaneous requests to the same endpoint |
| Resource exhaustion | DB connection pool full |
| Unexpected data | Valid but extreme values (empty array, max int, Unicode) |
| Partial failure | Service returns 500 on the 3rd of 5 required calls |
| State corruption | Start with pre-corrupted state, verify recovery |

---

## Test Quality Checklist

Before marking a task done:

- [ ] Every test has a clear name that describes the behavior, not the implementation
- [ ] No test is checking implementation details (only observable behavior)
- [ ] Confidence score ≥ 80% on Specificity and Coverage
- [ ] At least one mutation identified per test that correctly breaks it
- [ ] Chaos scenarios added for High Complexity tasks

---

## Never Delete Tests

If a test is annoying: fix the code, not the test. If a test seems wrong: understand it, don't delete it.

Exception: a test that proves nothing (confidence < 40% on coverage) should be rewritten, not preserved.

---

## Red Flags

- "The tests pass, so we're done" → Run the mutation check. Passing isn't proving.
- "I'll add tests after the implementation" → Tests written after are too easy to pass. Write them first.
- "This is too simple to test" → Simple things become complex. Write the test. It takes 2 minutes.
- "The test is a bit flaky sometimes" → Flaky tests are lying tests. Fix or delete.
