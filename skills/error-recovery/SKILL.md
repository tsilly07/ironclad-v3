---
name: error-recovery
description: Use when stuck in a loop, producing repeated errors, hitting the same bug 3+ times, or when a fix creates new problems — structured recovery before wasting more tokens
---

# Error Recovery

**Core principle:** When you're stuck, stop digging. Step back, reassess, recover.

## When to Use

- Same error appearing 3+ times despite fixes
- Fix creates new bug → fix that → creates another (cascade)
- Agent output is getting repetitive or circular
- Context window filling up with failed attempts
- Human partner says "stop" or "this isn't working"

## The Recovery Process

### 1. STOP — Acknowledge the Problem

Don't try one more fix. State clearly:
- What you were trying to do
- How many attempts you've made
- What's failing and why your fixes aren't working

```
"I've attempted 3 fixes for [problem] and none have resolved it. 
I'm going to step back and reassess rather than continue the same approach."
```

### 2. DIAGNOSE — Classify the Stuck Type

| Type | Signal | Recovery |
|------|--------|----------|
| **Loop** | Same error, same fix, repeat | Change approach entirely |
| **Cascade** | Each fix breaks something new | Revert to last working state |
| **Confusion** | Don't understand the error | Ask human, read docs, gather more context |
| **Scope creep** | Fix requires changing too much | Break into smaller problem |
| **Wrong assumption** | Code logic seems right but fails | Challenge your assumptions about how the system works |

### 3. REVERT — Get to Known Good State

Before trying anything new:

```bash
# Option A: Git revert to last working commit
git stash
git log --oneline -5  # Find last good commit

# Option B: Undo just the failed changes
git diff  # See what changed
git checkout -- <broken-files>
```

**If no git:** Tell human partner what to revert manually.

### 4. REASSESS — Fresh Approach

With clean state, ask yourself:
- Is the original approach fundamentally wrong?
- Am I missing a dependency or prerequisite?
- Is there a simpler way to achieve the same goal?
- Do I need information I don't have?

### 5. RECOVER — Try Different Strategy

Pick ONE of:

**Simplify:** Strip the implementation to bare minimum. Get that working. Add complexity incrementally.

**Research:** Ask human for docs, examples, or working code that does something similar.

**Decompose:** Break the problem into 2-3 smaller problems. Solve each independently.

**Escalate:** Tell human partner: "I need help with this specific part. Here's what I've tried and why it failed."

## Token Conservation

When stuck, every failed attempt wastes tokens. The recovery process saves tokens by:
- Stopping the bleeding (no more wasted attempts)
- Reverting to clean state (no accumulated mess)
- Changing strategy (not repeating failure)

**Rule of thumb:** If you've spent more tokens on fixes than on the original implementation, you need error recovery.

## Auto-Triggers

The agent should automatically enter error recovery when:
- 3rd attempt at the same fix
- Human says "stop", "this isn't working", "try something else"
- The same error message appears 3+ times in conversation
- A fix introduces more errors than it solves

## Red Flags

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

- "Just one more try" → You said that 2 tries ago. STOP.
- "I almost have it" → If you almost had it, you'd have it. Reassess.
- "This should work" → It didn't the last 3 times. Change approach.
- "Let me try a small tweak" → Small tweaks on a broken approach = more waste.
