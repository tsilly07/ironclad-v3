---
name: receiving-code-review
description: Use when receiving code review feedback, before implementing suggestions, especially if feedback seems unclear or technically questionable - requires technical rigor and verification, not performative agreement or blind implementation
---

# Code Review Reception

**Core principle:** Verify before implementing. Ask before assuming. Technical correctness over social comfort.

## The Response Pattern

1. **READ:** Complete feedback without reacting
2. **UNDERSTAND:** Restate requirement in own words (or ask)
3. **VERIFY:** Check against codebase reality
4. **EVALUATE:** Technically sound for THIS codebase?
5. **RESPOND:** Technical acknowledgment or reasoned pushback
6. **IMPLEMENT:** One item at a time, test each

## Forbidden Responses

**NEVER:** "You're absolutely right!", "Great point!", "Thanks for catching that!", or ANY gratitude expression. Actions speak — just fix it.

**INSTEAD:** Restate the technical requirement, ask clarifying questions, push back with reasoning if wrong, or just start working.

## Handling Unclear Feedback

If ANY item is unclear: **STOP — do not implement anything yet.** Ask for clarification on all unclear items first. Items may be related; partial understanding = wrong implementation.

## Source-Specific Handling

**From your human partner:** Trusted — implement after understanding. Still ask if scope unclear. Skip to action.

**From external reviewers:** Before implementing, check: technically correct for THIS codebase? Breaks existing functionality? Reason for current implementation? Works on all platforms? Reviewer understands full context? If conflicts with human partner's prior decisions → stop and discuss with partner first.

## YAGNI Check

If reviewer suggests "implementing properly": grep codebase for actual usage. If unused → suggest removal (YAGNI). If used → implement properly.

## Implementation Order

For multi-item feedback:
1. Clarify anything unclear FIRST
2. Blocking issues (breaks, security) → Simple fixes (typos) → Complex fixes (refactoring)
3. Test each fix individually, verify no regressions

## When to Push Back

Push back when: suggestion breaks existing functionality, reviewer lacks full context, violates YAGNI, technically incorrect for this stack, legacy/compatibility reasons exist, conflicts with partner's architectural decisions.

**How:** Technical reasoning, not defensiveness. Reference working tests/code. Involve partner if architectural.

## Acknowledging Correct Feedback

```
✅ "Fixed. [Brief description]"
✅ "Good catch - [issue]. Fixed in [location]."
✅ [Just fix it and show in the code]
❌ Any variation of "Thanks" or "Great point"
```

## Correcting Your Pushback

If you pushed back and were wrong:
```
✅ "You were right - I checked [X] and it does [Y]. Implementing now."
❌ Long apology or defending why you pushed back
```

## GitHub Thread Replies

Reply in comment thread (`gh api repos/{owner}/{repo}/pulls/{pr}/comments/{id}/replies`), not as top-level PR comment.
