---
name: review-response
description: Use when receiving code review feedback — triages findings by severity and root cause type, distinguishes fix-now from track-as-debt from disagree, and produces a structured response plan.
---

# Review Response

**Core principle:** All feedback is not equal. Triage before acting.

---

## The Problem With Undifferentiated Response

Treating every piece of review feedback as equally urgent produces:
- Wasted time on stylistic nits before fixing logical bugs
- Disagreements handled the same as factual errors
- Technical debt silently accumulated in "I'll do it later" promises

This skill brings structure to the chaos.

---

## Step 1 — Ingest Without Reacting

Read every piece of feedback before acting on any of it. Build a complete picture first.

Do not:
- Start fixing while still reading
- Defend before you've fully understood
- Agree reflexively to avoid conflict

---

## Step 2 — Classify Each Finding

For each piece of feedback, classify it across two axes:

### Axis 1: Severity

| Level | Definition |
|---|---|
| 🔴 Critical | Correctness bug, security issue, data loss risk, or requirement miss. Blocks ship. |
| 🟡 Significant | Performance issue, design smell, or behavior edge case. Should fix, can negotiate. |
| 🟢 Improvement | Readability, style, naming, minor structure. Valuable but not blocking. |
| ⚪ Opinion | Preference-based feedback where correct answer is subjective. |

### Axis 2: Response Type

| Type | When | Action |
|---|---|---|
| **Fix Now** | 🔴 Critical findings, or 🟡 Significant with clear solution | Fix before ship-gate |
| **Track As Debt** | 🟡/🟢 findings where cost to fix now outweighs benefit | Create tracked item, note in ship-gate |
| **Disagree** | Any finding where you believe the feedback is wrong | Articulate position clearly, escalate if needed |
| **Defer** | Valid but out of scope for this task | Add to backlog, out of current scope |
| **Accept** | Feedback is correct, no complexity, can fix immediately | Fix it |

---

## Step 3 — Build the Response Plan

Produce a triage table:

```markdown
## Review Response Plan — [Task Name]
Reviewer: [name/role]
Date: YYYY-MM-DD

| # | Finding | Severity | Response Type | Action | Notes |
|---|---|---|---|---|---|
| 1 | Null check missing on userId | 🔴 Critical | Fix Now | Fixed in commit abc123 | Was unintentional |
| 2 | N+1 query on user lookup | 🟡 Significant | Track As Debt | Issue #45 created | Requires cache layer design |
| 3 | Variable name `x` unclear | 🟢 Improvement | Accept | Renamed to `activeUserId` | — |
| 4 | Should use functional style here | ⚪ Opinion | Disagree | Imperative is intentional here for readability | See note |
```

---

## Step 4 — Handle Disagreements With Care

Disagreements are healthy and normal. Handle them properly:

1. **Articulate the reason**, not just the position:
   ```
   "I'm keeping the imperative style here because this section is read by non-experts 
   and the functional version obscures intent. Readability outweighs brevity at this 
   abstraction level."
   ```

2. **Offer to defer to human judgment** if the disagreement can't be resolved:
   ```
   "I see two valid positions here. I recommend [X] because [Y]. 
   Happy to defer if you prefer [Z]."
   ```

3. **Never defend without engaging**:
   ```
   Wrong: "I disagree."
   Right: "I disagree because... and here's the tradeoff I see..."
   ```

---

## Step 5 — Close the Loop

After acting on the response plan:

1. Update the review document with resolution status
2. Reference commit hashes or issue numbers for each action
3. Confirm with the reviewer that 🔴 Critical findings are resolved
4. Update ship-gate evidence to include review resolution

---

## What Not To Do

| Anti-Pattern | Why It's Wrong |
|---|---|
| Fix everything without triage | Wastes time on nits before bugs |
| Agree to avoid conflict | Unexamined agreement creates tech debt |
| Defend without articulating | Sounds dismissive, not reasoned |
| "I'll fix it later" without tracking | Later doesn't exist without a tracked item |
| Treat all opinions as facts | ⚪ Opinion feedback is a discussion, not an instruction |

---

## Red Flags

- Spending more time on 🟢 Improvements than 🔴 Criticals → triage first
- Responding to feedback in the order received → classify first, prioritize, then act
- "The reviewer is wrong" with no reasoning → articulate the disagreement properly
- Debt items with no issue number → they'll never get fixed
