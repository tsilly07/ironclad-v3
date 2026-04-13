---
name: ship-gate
description: Use before declaring any task, feature, or fix complete — produces a formal sign-off artifact with attached evidence. "Done" is a claim. Ship-gate is the proof.
---

# Ship Gate

**Core principle:** Done is not a feeling. Done is evidence.

---

## The Problem

"I think it's working" is not done.  
"Tests pass" is not done.  
"Looks good to me" is not done.

Done means every claim about the implementation is backed by observable, reproducible evidence — documented so anyone can verify it independently.

---

## When to Use

- Before marking any task complete
- Before merging into a main/release branch
- Before deploying to production
- When a human partner asks "is this ready?"

---

## The Six Evidence Requirements

Every ship gate sign-off must address all six:

### 1. Functional Evidence
Prove the feature works as intended.

```markdown
- [ ] Primary use case passes: [specific test or demo]
- [ ] Error paths handled: [which errors, how verified]
- [ ] Edge cases tested: [list with outcome]
```

### 2. Test Evidence
Prove the test suite is meaningful.

```markdown
- [ ] All tests pass: [test runner output or CI link]
- [ ] Test coverage includes: [specific behaviors, not just percentages]
- [ ] No tests were weakened or deleted to make the suite pass
- [ ] Confidence scores ≥ 80% on all critical tests (confidence-based-tdd)
```

### 3. Adversarial Review Evidence
Prove the code was tried to be broken.

```markdown
- [ ] adversarial-review completed: [link to review output]
- [ ] All 🔴 critical findings resolved
- [ ] All 🟡 warnings either resolved or explicitly tracked
```

### 4. Integration Evidence
Prove this works with everything around it.

```markdown
- [ ] Integration tests pass (not just unit tests)
- [ ] No regressions in adjacent functionality
- [ ] External dependencies tested (API contracts, DB queries, events)
```

### 5. Rollback Evidence
Prove you can undo this if it goes wrong.

```markdown
- [ ] Rollback path identified: [how to revert]
- [ ] Rollback tested: [or marked as untested with risk level]
- [ ] Data migration: [reversible / irreversible — if irreversible, backup confirmed]
```

### 6. Documentation Evidence
Prove the next person won't be lost.

```markdown
- [ ] Inline comments on non-obvious logic
- [ ] API changes documented in relevant docs
- [ ] Any new environment variables or config documented
- [ ] Breaking changes noted in CHANGELOG or commit message
```

---

## The Sign-Off Artifact

Save to `docs/ironclad/sign-offs/YYYY-MM-DD-<task>-shipgate.md`:

```markdown
# Ship Gate Sign-Off — [Task Name]
Date: YYYY-MM-DD
Task: [Task ID + name]
Author: [agent or human]

## Evidence Summary

| Requirement | Status | Evidence |
|---|---|---|
| Functional | ✅ | Manual test passed + see test-task-14.md |
| Tests | ✅ | 47 tests, 0 failures. Coverage: auth flow, error paths, edge cases |
| Adversarial Review | ✅ | 2 warnings tracked as issues #23, #24 |
| Integration | ✅ | E2E test suite passed in staging |
| Rollback | ⚠️ | Rollback script exists, not tested. Risk: Low |
| Documentation | ✅ | PR description + inline comments added |

## Decision

☑ SHIP — All critical evidence present. Warnings tracked.
☐ HOLD — Missing: [list what's missing]
☐ ESCALATE — Requires human decision on: [specific decision]
```

---

## When Ship Gate Fails

If any evidence requirement cannot be met:

- **Functional evidence missing**: The feature doesn't work yet. Do not ship.
- **Test evidence missing**: Write the tests. This is not optional.
- **Adversarial review missing**: Run it. Takes 20 minutes. Saves 20 hours.
- **Rollback missing**: Define it. Even "delete the new files" counts.
- **Docs missing**: Write one sentence per missing item. That's enough.

Partial evidence is not evidence. A half-completed ship gate is a failed ship gate.

---

## Escalation Triggers

Escalate to human before shipping if:
- Any rollback is marked irreversible
- Adversarial review found a 🔴 critical finding that can't be fixed in-scope
- Integration tests reveal unexpected behavior in adjacent systems
- Functional evidence contradicts the original intention-mapping spec

---

## Red Flags

- "I'm pretty confident it works" → Evidence or it didn't happen.
- "We can fix issues in prod" → Production fixes cost 10x. The gate exists to prevent this.
- "The tests pass, that's enough" → Tests alone satisfy only 1 of 6 requirements.
- "This is a small change, gate is overkill" → Small changes break things. Scale the depth, not the presence.
