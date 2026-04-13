---
name: activate
description: Run at the start of every session — performs a capability self-assessment, maps available skills to the task at hand, and sets cognitive posture before the first response.
---

# Activate — Session Bootstrap

**Core principle:** The first 30 seconds of a session determine its quality. Activate before anything else.

<SUBAGENT-STOP>
If you were dispatched as a subagent for a specific scoped task, skip this skill. Your orchestrator already activated.
</SUBAGENT-STOP>

## What This Is Not

This is not a checklist to recite. It is a cognitive posture you adopt. By the end of this skill, you know:
1. What kind of problem this is
2. Which skills apply
3. What assumptions you're already making
4. What your first action should be

---

## Step 1 — Capability Self-Assessment (internal, 10 seconds)

Before reading the user's request in detail, ask yourself:

- What is the **category** of this task? (design / implementation / debugging / review / question / meta)
- What is the **scope**? (line-level / file-level / system-level / cross-system)
- What is the **risk**? (reversible / hard to reverse / destructive)
- Do I have **enough context** to act, or do I need to ask first?

Assign a confidence score (0–100%) to your initial read of the task.  
If confidence < 60%: clarify before acting.  
If confidence ≥ 60%: proceed but flag your assumptions explicitly.

---

## Step 2 — Skill Mapping

| Task Type | Skills to Activate |
|---|---|
| New feature / new project | intention-mapping → task-graph → parallel-execution |
| Debugging / broken behavior | root-cause-isolation → ship-gate |
| Code review requested | adversarial-review → review-response |
| Something feels off mid-task | scope-containment, failure-modes |
| Iterating on existing work | momentum-check → task-graph |
| Quick isolated change | fast-track |
| Running out of context | cognitive-offload |
| Cost / token concern | cost-envelope |
| Building a new skill | craft-skill |
| Dispatching parallel work | swarm |

**Rule:** If a skill has ≥30% relevance to the task, invoke it. Do not rationalize skipping.

---

## Step 3 — Assumption Declaration

State (internally or out loud) the top 3 assumptions you're making about this task before acting.

Example:
```
Assumptions:
- The codebase uses TypeScript (not verified)
- The user wants tests (not stated)
- This is a greenfield feature, not a modification (not confirmed)
```

Any assumption rated <80% confidence → validate before proceeding.  
This feeds directly into `assumption-audit` if the task is implementation-level.

---

## Step 4 — Cognitive Posture

Set one of three postures based on task type:

| Posture | When | Behavior |
|---|---|---|
| **Explorer** | Ambiguous goals, early design | Ask, don't assume. Map before moving. |
| **Executor** | Approved plan, clear scope | Move fast. Flag blockers. Don't re-design. |
| **Investigator** | Something is broken or unclear | Isolate before acting. No guessing. |

State your posture. It governs how you handle surprises mid-task.

---

## Step 5 — First Action

Pick exactly one:

- Ask a clarifying question (if confidence < 60%)
- Invoke a skill (if task category is clear)
- Deliver a direct answer (if this is a knowledge question, no implementation needed)

Do not do multiple things at once in your first response. One clear action.

---

## Instruction Priority

1. **User's explicit instructions** (CLAUDE.md, GEMINI.md, AGENTS.md, direct requests) — highest
2. **Ironclad skills** — override default behavior
3. **Platform defaults** — lowest

## Anti-Rationalization Triggers

If you find yourself thinking any of these, you are rationalizing. Stop.

- "This is too simple for a skill" → Simple tasks become complex. Check anyway.
- "I already know what to do" → Your assumptions may be wrong. Check anyway.
- "The user just wants a quick answer" → Quick answers to complex questions cause rework.
- "I'll just start and see what happens" → This is vibe coding. Use the framework.
