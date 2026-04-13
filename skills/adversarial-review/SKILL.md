---
name: adversarial-review
description: Use when code is ready for review — the agent plays devil's advocate and actively tries to break the implementation across 5 dimensions. Not "does this look good" but "how can this fail".
---

# Adversarial Review

**Core principle:** A review that only looks for what's right is not a review. Find what breaks it.

---

## This Is Not a Checklist Review

Traditional code review asks: "Does this follow the spec? Does it look clean?" Adversarial review asks: "How would I break this if I were trying to?"

You are the attacker. Your own code is the target.

---

## When to Use

- Before marking any task complete
- Before merging a feature branch
- When a human partner asks for a code review
- After any High Complexity task from task-graph

---

## The Five Attack Dimensions

Work through all five. Do not skip dimensions because you expect them to be clean.

---

### Dimension 1 — Correctness Attack

Try to produce wrong output with valid input.

```
Questions to ask:
- Does this handle the empty case?
- Does this handle the maximum case?
- Does this handle duplicate/repeated calls?
- Is integer overflow or float precision possible?
- Is there an off-by-one error hiding here?
- Does the order of operations matter? Is it guaranteed?
```

Produce at least 3 concrete test inputs that you believe *should* work. Run them mentally or actually.

---

### Dimension 2 — Edge Case Attack

Try to produce unexpected behavior with unusual but valid input.

```
Questions to ask:
- What happens with null / undefined / empty string / zero?
- What happens with Unicode, special characters, or non-ASCII?
- What happens with extremely large or extremely small values?
- What if the user passes the maximum allowed payload size?
- What if two events fire simultaneously?
- What if a required resource is temporarily unavailable?
```

---

### Dimension 3 — Security Attack

Try to break the security model.

```
Questions to ask:
- Can a user access another user's data through this?
- Is any user input reflected back unsanitized?
- Are there hardcoded secrets, tokens, or credentials?
- Can this be called without proper authorization?
- Does this expose more information in errors than it should?
- Is rate limiting applied? Can this be abused with high frequency?
```

For non-security-sensitive code, this dimension can be brief. But don't skip it.

---

### Dimension 4 — Performance Attack

Try to make this code slow or resource-intensive.

```
Questions to ask:
- Is there an N+1 query lurking in a loop?
- Does this load entire datasets into memory instead of streaming?
- Is this O(n²) when O(n log n) is available?
- Does this call an expensive operation on every render/request?
- Can this cause a thundering herd under load?
```

---

### Dimension 5 — Maintainability Attack

Try to make the *next developer* (or future you) fail.

```
Questions to ask:
- If I deleted all comments, is this still understandable?
- Is this function doing more than one thing?
- Is the naming misleading? (e.g., `handleUser` that actually deletes users)
- Is there magic behavior / hidden coupling?
- If requirements change slightly, does this collapse or adapt?
- Will this break in 6 months when a dependency updates?
```

---

## Review Output Format

Document findings by severity:

```markdown
## Adversarial Review — [Task/Feature Name]

### 🔴 Critical (must fix before proceeding)
- [Dimension 2] No null check on `userId` — crashes if session expires mid-request

### 🟡 Warning (should fix, can unblock if tracked)
- [Dimension 4] Loading all records for pagination — will degrade past 10k records

### 🟢 Note (low severity, no blocking)
- [Dimension 5] `processData` function has 4 responsibilities — consider splitting

### ✅ Dimensions Passed
- Correctness: 5 test cases passed mentally
- Security: No user input reflected, auth checked at entry point
```

---

## Self-Review vs Peer Review

This skill applies to **self-review** — the agent reviewing its own work before declaring it done.

If a human partner is doing the review, invoke `review-response` to handle incoming feedback, not this skill.

---

## Blocking Rules

- 🔴 Critical issues: block `ship-gate`. Must fix.
- 🟡 Warning issues: agent may proceed but must create a tracked item
- 🟢 Notes: no blocking, logged for maintainability health

---

## Red Flags

- "I just wrote it, it should be fine" → You are blind to your own assumptions. Attack it.
- "The tests pass" → Tests prove assumptions. Attack the assumptions.
- "I'll catch it in QA" → QA finds user-visible bugs. This finds architectural ones.
- "It's just a small change" → Small changes break large systems. Review it.
