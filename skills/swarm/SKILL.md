---
name: swarm
description: Use when a task-graph has identified parallelizable work across multiple workstreams — models the full swarm as a system with task allocation, conflict arbitration, result aggregation, and failure handling for subagents.
---

# Swarm

**Core principle:** Parallelism is not just "run multiple things." It's a system with coordination, conflict resolution, and aggregation.

---

## When to Use

- task-graph has identified parallel phases with ≥3 independent tasks
- The work spans multiple independent domains (frontend + backend + tests simultaneously)
- Speed matters and tasks have clear, testable outputs
- The orchestrating agent can stay available to coordinate

Do not use swarm for:
- Tasks with tight interdependencies (use parallel-execution with phase sync instead)
- Tasks requiring shared mutable state without clear ownership
- Fewer than 3 parallel tasks (overhead not worth it)

---

## Swarm Architecture

```
Orchestrator (you)
├── Oversees the task graph
├── Allocates tasks to subagents
├── Monitors for conflicts
├── Arbitrates disputes
└── Aggregates results

Subagents
├── Each owns one task slice
├── Each works in isolation
├── Each produces a structured result artifact
└── Each reports on completion or blockage
```

---

## Step 1 — Task Allocation

Before dispatching, prepare a task packet for each subagent:

```markdown
## Subagent Task Packet — [Subagent Name / Task ID]

### Your Task
[T-ID]: [Task name and clear description]

### Inputs Available
- [List of files, APIs, or data this subagent can access]

### Outputs Expected
- [Specific files, artifacts, or state changes to produce]

### Test Criteria
- [Exact, verifiable: not "it works" but specific behavior]

### Scope Boundary
- These files ONLY: [list]
- Do NOT modify: [list]
- Do NOT wait for: [list of other concurrent tasks]

### On Completion
Return: status (done/blocked), summary of changes, evidence (test output)
Return on blockage: exact blocker, what's needed to unblock
```

Never dispatch a subagent without a full task packet. Incomplete packets produce incomplete results.

---

## Step 2 — Conflict Prevention (Pre-Dispatch)

Before dispatching, audit for potential conflicts:

**File conflicts:**  
Two subagents assigned to modify the same file → serialize those specific tasks, not the whole swarm.

**State conflicts:**  
Two subagents that write to the same database table or shared state → define write ownership. One owns the write, others read.

**Interface conflicts:**  
Subagent A defines an interface that Subagent B consumes → define interface contract before dispatch, lock it, don't let either agent change it mid-flight.

---

## Step 3 — Dispatch

Dispatch all non-conflicting tasks simultaneously. Record dispatch time and expected completion.

```markdown
## Swarm Dispatch Log

| Subagent | Task | Dispatched | Expected | Status |
|---|---|---|---|---|
| Alpha | T-01: Auth module | 10:00 | 10:30 | 🔄 Running |
| Beta | T-02: User schema | 10:00 | 10:20 | 🔄 Running |
| Gamma | T-06: Email service | 10:00 | 10:45 | 🔄 Running |
```

---

## Step 4 — Monitoring and Arbitration

While subagents run, monitor for:

**Completion reports:** Accept when all test criteria are met. Reject (request retry) if evidence is missing.

**Blockage reports:** When a subagent reports a blocker:
1. Assess: is this in-scope or out-of-scope?
2. In-scope: resolve as orchestrator, unblock
3. Out-of-scope: escalate to human
4. If blocker affects other subagents: pause affected agents

**Conflict reports:** When two subagents report conflicting changes:
1. Stop both
2. Arbitrate: which change takes precedence? (usually: the one aligned with task-graph order)
3. Instruct winner to proceed, instruct loser to adapt to winner's output
4. Log the conflict and resolution

---

## Step 5 — Result Aggregation

When all subagents complete:

1. Collect all result artifacts
2. Run integration tests (unit tests already ran per subagent — now test the whole)
3. Resolve any merge conflicts using the conflict arbitration protocol
4. Produce a swarm summary:

```markdown
## Swarm Summary — [Feature Name]

### Results
| Agent | Task | Status | Evidence |
|---|---|---|---|
| Alpha | T-01 | ✅ Done | 12 tests passing |
| Beta | T-02 | ✅ Done | Schema migration tested |
| Gamma | T-06 | ⚠️ Partial | Email sending works, template missing |

### Conflicts Resolved
- Beta/Alpha file conflict on `user.model.ts`: Alpha output preserved, Beta adapted

### Outstanding Issues
- Gamma: email template T-06a created as follow-up task

### Integration Test Result
All integration tests pass. Ready for ship-gate.
```

---

## Failure Handling

If a subagent fails to complete:

| Scenario | Action |
|---|---|
| Subagent stuck (same attempt 3x) | Reassign task to orchestrator or new subagent |
| Subagent produced wrong output | Reject result, provide corrected task packet, re-dispatch |
| Subagent broke shared state | Revert shared state, re-run affected tasks serially |
| Multiple subagents failed | Abort swarm, fall back to sequential execution |

Never silently accept a failed subagent result. Every result must be validated before aggregation.

---

## Red Flags

- "I'll just let subagents figure out conflicts" → Conflicts must be prevented at allocation or arbitrated explicitly.
- Dispatching without a task packet → Subagents will make assumptions. Most will be wrong.
- "Swarm is faster, always use it" → Overhead + coordination cost is real. Use it only when parallel gain outweighs coordination cost (≥3 independent tasks).
