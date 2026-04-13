---
name: dispatching-parallel-agents
description: Use when facing 2+ independent tasks that can be worked on without shared state or sequential dependencies
---

# Dispatching Parallel Agents

You delegate tasks to specialized agents with isolated context. By precisely crafting their instructions, you ensure they stay focused. They never inherit your session's context — you construct exactly what they need.

**Core principle:** Dispatch one agent per independent problem domain. Let them work concurrently.

## When to Use

**Use when:** 3+ test files/subsystems failing with different root causes, each problem is independent, no shared state between investigations.

**Don't use when:** Failures are related (fix one might fix others), need full system context, agents would interfere (editing same files).

## The Pattern

### 1. Identify Independent Domains
Group failures by what's broken. Each domain must be independent — fixing one shouldn't affect others.

### 2. Create Focused Agent Tasks
Each agent gets: specific scope (one file/subsystem), clear goal, constraints (don't change other code), expected output format.

### 3. Dispatch in Parallel
```typescript
Task("Fix agent-tool-abort.test.ts failures")
Task("Fix batch-completion-behavior.test.ts failures")
Task("Fix tool-approval-race-conditions.test.ts failures")
```

### 4. Review and Integrate
Read each summary → verify fixes don't conflict → run full test suite → integrate.

## Agent Prompt Structure

Good prompts are **focused** (one problem), **self-contained** (all context included), **specific about output** (what to return).

```markdown
Fix the 3 failing tests in src/agents/agent-tool-abort.test.ts:

1. "should abort tool with partial output" - expects 'interrupted at' in message
2. "should handle mixed completed and aborted" - fast tool aborted instead of completed
3. "should track pendingToolCount" - expects 3 results but gets 0

These are timing/race condition issues. Your task:
1. Read test file, understand what each test verifies
2. Identify root cause - timing issues or actual bugs?
3. Fix by replacing timeouts with event-based waiting

Do NOT just increase timeouts - find the real issue.
Return: Summary of root cause and changes.
```

## Common Mistakes

- **Too broad** ("Fix all the tests") → Be specific to one file/subsystem
- **No context** (no error messages) → Paste errors and test names
- **No constraints** → Specify what NOT to change
- **Vague output** ("Fix it") → Request summary of root cause + changes

## Verification

After agents return:
1. Review each summary
2. Check for conflicts (same code edited?)
3. Run full suite
4. Spot check — agents can make systematic errors
