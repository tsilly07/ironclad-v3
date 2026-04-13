---
name: intention-mapping
description: Use before any implementation work — models the user's goal as a dependency graph, surfaces hidden constraints, and finds the actual goal vs the stated goal before a single line is planned.
---

# Intention Mapping

**Core principle:** People state solutions, not problems. Your job before anything else is to find the real problem.

<HARD-GATE>
Do NOT plan, design, or write any code until an intention map is approved. This applies to every task regardless of apparent simplicity.
</HARD-GATE>

---

## The Problem With Brainstorming

Traditional brainstorming asks "what do you want?" and then builds it. This produces solutions to the wrong problem. Intention mapping asks "why do you want this?" until the real goal surfaces.

---

## Step 1 — Stated vs Actual Goal

Extract the stated goal from the user's request. Then challenge it with the **5-layer drill**:

```
Layer 1: What did they ask for?
Layer 2: What outcome are they trying to achieve?
Layer 3: What problem makes that outcome necessary?
Layer 4: What constraint makes that problem exist?
Layer 5: Is there a simpler path to the Layer 2 outcome?
```

If Layer 5 reveals a simpler path: present it. The user may not have considered it.

**Example:**  
"Add a caching layer" →  
Layer 2: Faster response times  
Layer 5: Have you profiled? Is caching actually the bottleneck?

---

## Step 2 — Constraint Discovery

Map all constraints before proposing anything. Ask one at a time:

| Constraint Type | Questions to Ask |
|---|---|
| **Hard constraints** | What absolutely cannot change? (stack, deployment, existing contracts) |
| **Soft constraints** | What are you hoping to avoid, but could change? |
| **Time constraints** | Is there a deadline driving this? |
| **Quality constraints** | What does "done" look like? What would make this a failure? |
| **Unstated constraints** | What are you assuming I'll preserve that you haven't mentioned? |

---

## Step 3 — Build the Intention Map

Draw the goal as a dependency graph (can be text-based):

```
[Root goal]
├── [Sub-goal A] — depends on: nothing
│   ├── [Requirement A1]
│   └── [Requirement A2] — conflicts with: [Sub-goal B]
├── [Sub-goal B]
│   └── [Requirement B1] — assumption: X is true (confidence: 70%)
└── [Hidden goal C] — discovered in layer drill
    └── [Requirement C1] — simpler path exists
```

Flag:
- **Conflicts**: goals that pull in opposite directions
- **Assumptions**: things you're assuming are true (with confidence %)
- **Blockers**: things that must be resolved before proceeding
- **Simplifications**: goals that can be achieved with less than asked

---

## Step 4 — Propose Approaches (2–3 max)

Only after the map is approved. Each approach must state:

1. Which parts of the intention map it addresses
2. Which constraints it respects vs trades off
3. Estimated complexity (not time — complexity: Low / Medium / High / Extreme)
4. Your recommendation and why

**Anti-pattern:** "Here are 5 options." More than 3 = you haven't thought hard enough about the tradeoffs.

---

## Step 5 — Design Document

Save to `docs/ironclad/specs/YYYY-MM-DD-<topic>-intention.md`.

Include:
- Root goal + actual goal (if different)
- Full intention map (text DAG)
- Constraints (hard + soft)
- Chosen approach + rationale
- Risks and open questions

Commit it. It becomes the source of truth for task-graph.

---

## Step 6 — Self-Review Before Moving On

Check the spec for:

- [ ] Any "and" in a requirement (= two requirements, split them)
- [ ] Any word like "simple", "just", "obviously" (= hidden complexity)
- [ ] Any requirement with no acceptance criterion
- [ ] Any assumption with <80% confidence (must validate before task-graph)

Only when self-review passes → invoke `task-graph`. No other skill.

---

## One Question at a Time

Never ask compound questions. One question → answer → next question. Move faster by going deeper, not broader.

## Red Flags

- "The user's request is pretty clear" → The stated goal is clear. The actual goal may not be.
- "I've built this before" → This context is different. Map it.
- "Let me propose approaches first, then ask questions" → Map before proposing. Always.
