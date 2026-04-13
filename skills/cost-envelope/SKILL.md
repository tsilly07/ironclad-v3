---
name: cost-envelope
description: Use when starting any project or when costs are a concern — models total execution cost across multiple dimensions (compute, time, complexity debt, coordination) beyond token counting alone.
---

# Cost Envelope

**Core principle:** Token cost is the cheapest cost. The real costs are complexity debt and time.

---

## Why Token Counting Is Insufficient

Token budgets tell you one number: API spend. They miss:
- **Complexity debt**: How hard is this to maintain, extend, or debug later?
- **Time cost**: How long does this take, in wall time?
- **Coordination cost**: How many people / agents need to be aligned for this?
- **Opportunity cost**: What are you NOT building because you're doing this?
- **Rollback cost**: How hard is it to undo this if it goes wrong?

A full cost envelope models all five.

---

## The Five Cost Dimensions

### 1. Compute Cost (tokens / API spend)
- Estimated tokens per task tier
- Running total as project progresses
- Alert threshold: 75% of budget

| Tier | Tokens | Use When |
|---|---|---|
| Minimal | ~30K | Config changes, small isolated fixes |
| Standard | ~150K | Most features |
| Thorough | ~400K | Complex systems, high-stakes code |
| Intensive | ~800K+ | Full feature with design, TDD, review, integration |

### 2. Complexity Debt
How much harder does this make the system to work with in the future?

Rate each design decision:
- **Additive**: adds complexity that will need managing
- **Neutral**: doesn't change system complexity
- **Reductive**: removes complexity (rare, valuable)

```markdown
Decision: Add caching via Redis
Complexity debt: ADDITIVE
- Cache invalidation logic to maintain
- New failure mode: cache out of sync
- New dependency to manage
Debt score: +3 (1-10 scale)
```

Total project complexity debt score is tracked. High scores require a simplification pass.

### 3. Time Cost
Wall time, not token time.

- How long does this take in real time to execute?
- Does this block other work?
- Does this require human review that has a wait time?

Track: estimated hours vs actual hours per phase.

### 4. Coordination Cost
How many parties need to align for this work?

- Solo (agent alone): 0 coordination cost
- Agent + human review: low
- Multi-agent swarm: medium
- Cross-team: high
- External dependency release: very high

High coordination cost = high fragility. Build in buffers.

### 5. Rollback Cost
How hard is it to undo this?

| Level | Description | Actions Required |
|---|---|---|
| Free | Git revert | 1 command |
| Cheap | Schema migration revert | 2-3 steps, no data loss |
| Moderate | Data migration with backward migration | Multiple steps, test required |
| Expensive | Irreversible data change | Backup restore, downtime likely |
| Catastrophic | Production data loss possible | Do not proceed without full plan |

Any Expensive or Catastrophic rollback cost: mandatory human sign-off before proceeding.

---

## Cost Envelope Document

At project start, create `docs/ironclad/costs/YYYY-MM-DD-<project>-cost-envelope.md`:

```markdown
# Cost Envelope — [Project Name]

## Compute Budget
- Total: ~800K tokens (~$4 on Sonnet 4.6)
- Per phase: Phase 1 (200K), Phase 2 (300K), Phase 3 (300K)
- Alert: warn at 600K

## Complexity Debt Budget
- Starting score: 12 (existing system)
- Max acceptable: 20
- Current: 12

## Time Budget
- Estimated: 3 sessions
- Current: Session 1/3

## Coordination
- Parties: agent + 1 human reviewer
- Review gate: after Phase 2

## Rollback Assessment
- Phase 1: Free (no DB changes)
- Phase 2: Moderate (schema migration, reversible)
- Phase 3: Cheap (code only)
```

---

## Cost Alerts

Trigger a cost alert when:
- Compute hits 75% of budget
- Complexity debt score rises by 5+ on a single task
- Time cost is 2x estimated
- Coordination cost increases unexpectedly (new party added)
- Rollback cost increases (was Cheap, now Expensive)

On alert: pause. Present the cost picture to the human. Decide to proceed, descope, or re-plan.

---

## Cost-Saving Strategies

**Compute:**
- Reuse patterns from earlier tasks (don't re-discover)
- Batch related edits into one context
- Don't summarize unless asked — skip the preamble

**Complexity debt:**
- Prefer reversible decisions over clever ones
- YAGNI: don't build what you might need
- The simplest code that works is the right code

**Time:**
- Parallelize (use task-graph + swarm) — don't serialize what can run concurrently
- Pre-validate assumptions before execution, not during

**Coordination:**
- Front-load human review (one session, batch decisions) vs several scattered
- Async questions > sync meetings

---

## Red Flags

- "Let me be thorough and add X while I'm here" → That's unplanned compute + complexity debt. Is it budgeted?
- "This is a small change" → Check rollback cost. Small changes can have expensive rollbacks.
- "We'll refactor later" → Track the complexity debt now. "Later" needs to be a real task.
