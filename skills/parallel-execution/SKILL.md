---
name: parallel-execution
description: Use when executing a task-graph — orchestrates multiple workstreams simultaneously, manages inter-task dependencies in real time, and detects conflicts before they compound.
---

# Parallel Execution

**Core principle:** Sequential execution of parallelizable work is waste. Execute all independent branches simultaneously.

---

## Prerequisites

Before invoking this skill:
- task-graph has been built and approved
- All High Risk tasks have pre-planned recovery paths (failure-modes)
- All assumptions rated <80% have been validated (assumption-audit)

If any prerequisite is missing: stop and complete it.

---

## Execution Model

### Phase Execution

Execute the task-graph phase by phase. Within each phase, all tasks run in parallel. Between phases, synchronize.

```
Phase 1: Launch T-01, T-02, T-06 simultaneously
         → Wait for all to complete
         → Review phase outputs
         → Confirm no conflicts
Phase 2: Launch T-03, T-04 simultaneously (dependencies from Phase 1 met)
         → ...
```

### Single-Agent Mode

If running as one agent (not `swarm`):
- Simulate parallelism by context-switching between tasks
- Prioritize critical-path tasks first within each phase
- Don't context-switch mid-task — complete a task unit before switching

### Swarm Mode

If using `swarm` for true parallelism:
- Dispatch one subagent per parallel task
- Each subagent receives: task description, inputs, test criteria, relevant files only
- Main agent coordinates: tracks status, resolves conflicts, controls merge points

---

## Real-Time Conflict Detection

After each task completes, before starting the next phase, run a conflict check:

### File Conflict
Did two parallel tasks modify the same file? If yes:
1. Stop
2. Identify which change is correct
3. Merge manually
4. Re-run tests on the merged result

### Dependency Conflict
Did a completed task produce output that doesn't match what a pending task expects?
1. Stop
2. Identify the mismatch
3. Decide: fix the producer or update the consumer expectation
4. Document the deviation from the original task-graph

### State Conflict
Did two parallel tasks leave the system in an inconsistent state?
1. Revert both tasks
2. Identify the ordering constraint that was missed
3. Update the task-graph to make this sequential
4. Re-execute

---

## Progress Tracking

Maintain a live status board throughout execution:

```markdown
## Execution Status — [Feature Name]

### Phase 1
| Task | Status | Agent | Notes |
|---|---|---|---|
| T-01 | ✅ Done | main | — |
| T-02 | ✅ Done | main | Found edge case — logged as T-02a |
| T-06 | 🔄 In progress | main | — |

### Phase 2 (blocked until Phase 1 complete)
| Task | Status | Deps Met? |
|---|---|---|
| T-03 | ⏳ Waiting | T-01 ✅ |
| T-04 | ⏳ Waiting | T-01 ✅, T-02 ✅ |
```

---

## Deviation Protocol

When actual execution deviates from the task-graph:

1. **Minor deviation** (implementation detail only): Log it, continue
2. **Moderate deviation** (task scope changed): Pause, update task-graph, get confirmation
3. **Major deviation** (phase structure must change): Pause, notify human, re-plan

Never silently absorb a major deviation. It compounds.

---

## Merge Points (High Attention Required)

When parallel branches converge:
1. Pause all execution
2. Review outputs from all branches
3. Run integration tests (not just unit tests)
4. Confirm no conflicts
5. Get human confirmation if risk-rated High or Critical
6. Only then proceed to the next phase

Merge points are where most parallel execution failures occur. Give them full attention.

---

## Completion Evidence

When all phases complete:
- Full test suite passes (not just per-task tests)
- No open conflicts or deviations without resolution
- Progress board shows all tasks ✅
- Ready to feed into `ship-gate`

---

## Red Flags

- "I'll just do them one at a time to be safe" → Sequential = slower. Use phases.
- "These tasks are independent, I don't need to sync" → Independent tasks still produce shared state. Sync at merge points.
- "I'll handle conflicts if they happen" → Conflict detection is cheaper than post-hoc debugging.
