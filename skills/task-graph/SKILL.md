---
name: task-graph
description: Use after intention-mapping to convert an approved design into a directed acyclic graph of tasks — identifies parallelizable work, critical path, and sequencing dependencies before execution begins.
---

# Task Graph

**Core principle:** A flat task list is a lie. Real work has dependencies. Model them.

<HARD-GATE>
Do NOT begin execution until the task graph is built and the critical path is identified. A list of tasks without dependency modeling is not a plan — it is a wish list.
</HARD-GATE>

---

## Why Not a Flat List?

Flat task lists assume all tasks are sequential. They aren't. Task graphs:
- Expose what can run in parallel (faster execution, especially with `swarm`)
- Show the critical path (what blocks everything else)
- Make bottlenecks visible before they become blockers
- Enable smarter checkpointing

---

## Step 1 — Extract Tasks From the Intention Map

For each requirement in the intention map, derive 1–N tasks.

**Task quality rules:**
- One task = one testable outcome
- If a task contains "and" → split it
- A task should be completable in one focused session
- Every task has: ID, description, inputs, outputs, test criteria

```markdown
### Task T-01: [Name]
- **Inputs:** [files, data, state that must exist before this starts]
- **Outputs:** [files, state, artifacts produced]
- **Depends on:** [T-xx, T-yy]
- **Test criteria:** [specific, verifiable — not "it works"]
- **Complexity:** [Low / Medium / High]
- **Risk:** [what could go wrong]
```

---

## Step 2 — Build the Dependency Graph

Map all tasks and their dependencies. Use text DAG format:

```
T-01 (no deps)
T-02 (no deps)
T-03 depends on T-01
T-04 depends on T-01, T-02
T-05 depends on T-03, T-04
T-06 (no deps)
T-07 depends on T-05, T-06
```

Visualize as a graph. Explicitly mark:
- **Parallelizable sets**: tasks with no dependency on each other → these can run simultaneously via `swarm`
- **Sequential chains**: a → b → c → can't be parallelized
- **Merge points**: where parallel branches converge → high-risk, need careful coordination

---

## Step 3 — Identify the Critical Path

The critical path = the longest dependency chain from start to finish.

```
Critical Path: T-01 → T-03 → T-05 → T-07
Estimated length: 4 tasks
```

**Rule:** Anything on the critical path gets the most scrutiny. Any delay here delays everything.

If the critical path is more than 60% of all tasks: look for ways to break it. Can any sequential dependency be made parallel with a small design change?

---

## Step 4 — Flag Risk Tasks

For each task rated High Risk or High Complexity, add a **tripwire**:

```markdown
### T-04: Database migration
- Risk: HIGH — irreversible if botched
- Tripwire: Manual approval required before execution
- Pre-conditions: Full backup confirmed, staging test passed
- Rollback plan: Script at scripts/rollback-T04.sh
```

Risk tasks cannot be auto-executed by `parallel-execution`. They require human confirmation.

---

## Step 5 — Execution Order Proposal

Produce a concrete execution order that respects dependencies and maximizes parallelism:

```
Phase 1 (parallel): T-01, T-02, T-06
Phase 2 (parallel): T-03, T-04  [after T-01 and T-02]
Phase 3 (sequential): T-05  [after T-03 and T-04 — merge point, review required]
Phase 4 (sequential): T-07  [after T-05 and T-06]
```

---

## Step 6 — Save the Graph

Save to `docs/ironclad/plans/YYYY-MM-DD-<topic>-graph.md`.

Commit. This is the source of truth for `parallel-execution` and `swarm`.

---

## Checkpoints

Mandatory human checkpoints:
- After any merge point (parallel → sequential)
- Before any High Risk task
- When actual state diverges from planned state

Optional auto-proceed:
- Low/Medium complexity tasks with no risk flag
- Tasks with clear, passing test criteria

---

## Task Graph vs Flat Plan

| Flat Plan | Task Graph |
|---|---|
| Sequential by default | Parallel by design |
| No dependency tracking | Explicit deps + critical path |
| Bottlenecks found during execution | Bottlenecks visible before execution |
| No rollback planning | Rollback plan per risk task |
| Human can't audit ordering | Graph makes ordering auditable |

## Red Flags

- "Let me just list the tasks" → List without deps = no visibility. Build the graph.
- "I'll figure out the order as I go" → Critical path calculation takes 5 minutes. Skipping it costs hours.
- "These tasks are obviously sequential" → Confirm it. Many obvious sequences can be parallelized.
