# Ironclad

A cognitive framework for AI coding agents. Not a process enforcer — a judgment scaffolding system.

Ironclad doesn't tell agents to follow a workflow. It shapes *how they think* — surfacing assumptions, modeling failure before it happens, tracking scope in real time, and measuring velocity instead of activity.

It works with Claude Code, Cursor, Codex, Gemini CLI, OpenCode, and any platform that reads markdown skill files.

---

## Philosophy

- **Judgment over compliance** — Checklists fail at the edges. Judgment succeeds there.
- **Evidence over assertion** — "Done" requires proof, not declaration.
- **Prevention over recovery** — Map failure modes before they happen, not after.
- **Velocity over activity** — Effort is not progress. Measure what closes.
- **Lean by design** — Every line in every skill earned its place.

---

## What's Inside

### Core Workflow

| Skill | What Makes It Different |
|---|---|
| **activate** | Session bootstrap with capability self-assessment and cognitive posture setting — not just "load your skills" |
| **intention-mapping** | 5-layer goal drill to find the *actual* goal vs the stated one, modeled as a dependency graph |
| **task-graph** | DAG-based task planning with critical path, parallelism identification, and risk tripwires — not a flat list |
| **parallel-execution** | Phase-based orchestration with real-time conflict detection and merge point protocols |
| **confidence-based-tdd** | RED-GREEN-REFACTOR + confidence scoring, mutation testing, and chaos scenarios |
| **branch-strategy** | Full branch lifecycle: risk assessment, feature flags, merge windows, rollback planning |
| **completion-protocol** | Formal handoff with diff summary, migration plan, copy-paste rollback — not just merge/PR/discard |
| **fast-track** | Calibrated bypass for truly isolated ≤20-line changes, with qualification criteria and accumulation warnings |

### Quality Gates

| Skill | What Makes It Different |
|---|---|
| **adversarial-review** | Devil's advocate across 5 attack dimensions: correctness, edge cases, security, performance, maintainability |
| **review-response** | Triage feedback by severity + response type — fix-now / track-as-debt / disagree (with reasoning structure) |
| **root-cause-isolation** | Binary search on assumptions — eliminates half the assumption space per step instead of sequential guessing |
| **ship-gate** | 6 evidence requirements with a formal sign-off artifact — done is proof, not feeling |

### Ironclad Originals

| Skill | What It Is |
|---|---|
| **assumption-audit** | Surfaces and rates every assumption across 5 categories before implementation begins |
| **scope-containment** | Real-time scope creep detection with a 4-option protocol and a scope discovery log |
| **cognitive-offload** | Structured external memory in `.ironclad/memory/` — typed, tiered, queryable, not a flat state file |
| **failure-modes** | Pre-execution failure mapping with likelihood/impact ratings, tripwires, and pre-planned recovery paths |
| **cost-envelope** | 5-dimension cost modeling: compute, complexity debt, time, coordination, rollback — beyond token counting |
| **momentum-check** | Velocity score + quality audit + complexity trend after each execution cycle |

### Meta

| Skill | What It Is |
|---|---|
| **craft-skill** | Evidence-driven skill authoring — requires a worked example and real-case test before a skill is published |
| **swarm** | Full swarm coordination: task allocation packets, conflict arbitration, result aggregation, failure handling |

---

## Installation

### Claude Code
```
/plugin install ironclad
```

### Cursor
```
/add-plugin ironclad
```

### Gemini CLI
```
gemini extensions install https://github.com/tsilly07/ironclad-v3
```

### Codex / OpenCode
Copy the `skills/` directory into your workspace.

---

## How It Works

Every session starts with `activate`. It maps the task, sets cognitive posture, and identifies which skills apply. Then:

1. **Intention-mapping** — finds the real goal, maps constraints, documents the design
2. **Task-graph** — builds a DAG with parallelism and critical path identified
3. **Assumption-audit** — validates every assumption before a line is written
4. **Failure-modes** — maps how it can fail *before* it does
5. **Parallel-execution / Swarm** — executes the graph with real-time conflict detection
6. **Confidence-based TDD** — proves behavior, not just test passage
7. **Adversarial-review** — actively tries to break the implementation
8. **Ship-gate** — formal sign-off with attached evidence

**Scope-containment** runs continuously throughout. **Momentum-check** runs after each cycle.

---

## Design Principles

**Skills are working software, not documentation.** Every skill was tested on a real case before inclusion.

**One source of truth per concept.** No duplicated anti-patterns across 20 skills. One concept, one place.

**Adaptive depth.** Fast-track for trivial changes. Full workflow for complex ones. The framework scales.

**Platform-agnostic.** Markdown skill files work with any agent that can read them. Platform profiles handle the differences.

---

## License

MIT
