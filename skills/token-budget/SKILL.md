---
name: token-budget
description: Use when starting a project or large task to set token spending limits, track consumption per task, and prevent runaway costs — especially useful on API pay-as-you-go
---

# Token Budget

**Core principle:** Every token costs money. Track spending, set limits, and optimize before you run out.

## When to Use

- Starting any multi-task project on API billing
- Human partner has a budget constraint
- Tasks are consuming more tokens than expected
- Need to decide between thorough vs. fast approach

## Budget Tiers

Before starting a project, classify the approach:

| Tier | Approach | Token Use | When |
|------|----------|-----------|------|
| **Minimal** | Skip brainstorm, minimal planning, direct code | ~50K per resource | Simple configs, small fixes |
| **Standard** | Quick design, plan, TDD execution | ~150K per resource | Most custom resources |
| **Thorough** | Full brainstorm, detailed plan, full review cycle | ~300K per resource | Complex systems, critical code |

**Ask human partner which tier at project start.** Default to Standard.

## Per-Task Tracking

After each completed task, log in PROJECT_STATE.md:

```markdown
## Token Budget
- Tier: Standard
- Estimated total: ~1.5M tokens ($5-8 on Sonnet 4.6)
- Spent so far: ~400K tokens (~$2)
- Remaining: ~1.1M tokens

### Per Resource
| Resource | Estimated | Actual | Status |
|----------|-----------|--------|--------|
| custom-jobs | 150K | 120K | ✅ Done |
| drug-system | 200K | — | 🔄 Next |
| reputation | 200K | — | ⏳ Planned |
```

## Cost-Saving Strategies

### 1. Don't re-read files
If you already read a file this session, work from memory. Only re-read if it changed.

### 2. Batch related changes
Instead of 5 separate requests for 5 similar configs, do them all in one prompt.

### 3. Skip verbose output
When human partner doesn't need explanation, just output the code. No preambles, no summaries.

### 4. Reuse patterns
First resource takes more tokens (establishing patterns). Subsequent similar resources should reference the first instead of re-discovering patterns.

### 5. Tier down for simple tasks
Config file edit? That's Minimal tier. Don't run full brainstorm → plan → TDD for changing a number in a config.

## Budget Alerts

Agent should warn when:
- A single task exceeds 2x its tier estimate
- Total project spending reaches 75% of budget
- A retry loop is consuming tokens without progress (→ trigger error-recovery)

## Red Flags

- "Let me explain in detail what I'm about to do" → Just do it. Save tokens.
- "Let me re-read that file to make sure" → Did it change? No? Work from memory.
- "Here's a comprehensive summary of everything" → Human didn't ask for it. Skip.
- Running brainstorm for a 5-line config change → Tier down to Minimal.
