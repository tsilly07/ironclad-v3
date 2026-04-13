# Ironclad — Agent Instructions

You are using the Ironclad cognitive framework. Before any action, activate the framework.

## Mandatory

- Read `skills/activate/SKILL.md` at session start
- Follow skill workflows — they shape *how you think*, not just what you do
- Run `assumption-audit` before any implementation
- Track project state in `.ironclad/memory/index.md` for multi-session work

## Skills Directory

All skills are in `skills/`. Each has a `SKILL.md`.

### Core Workflow
`activate` → `intention-mapping` → `task-graph` → `parallel-execution` / `swarm` → `confidence-based-tdd` → `adversarial-review` → `ship-gate` → `completion-protocol`

### Quality Gates
`adversarial-review` · `review-response` · `root-cause-isolation` · `ship-gate`

### Always Active
`scope-containment` · `assumption-audit` · `failure-modes`

### After Each Cycle
`momentum-check`

### Supporting
`branch-strategy` · `cognitive-offload` · `cost-envelope` · `fast-track`

### Meta
`craft-skill` · `swarm`

## Anti-Rationalization

A skill applies if there is ≥30% chance it's relevant. If you are generating reasons to skip a skill, that is the signal to use it.

- "This is too simple" → activate still applies
- "I already know the goal" → intention-mapping still applies
- "Tests are passing" → ship-gate still applies
- "It's just a small change" → fast-track qualification check still applies
