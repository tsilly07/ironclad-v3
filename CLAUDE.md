# Ironclad — Claude Code Configuration

This project uses the Ironclad cognitive framework for AI coding agents.

## Setup

Begin every session by reading `skills/activate/SKILL.md`.

## Rules

- Invoke skills before responding — even 30% chance of relevance = invoke
- Run `assumption-audit` before any implementation work
- Maintain `.ironclad/memory/index.md` across sessions
- Use `fast-track` only when all qualification criteria are met
- `scope-containment` is always active during execution

## Skill Reference

| When | Use |
|---|---|
| Session start | `activate` |
| New feature / project | `intention-mapping` → `task-graph` |
| Before coding | `assumption-audit` + `failure-modes` |
| During execution | `parallel-execution` or `swarm` |
| Writing code | `confidence-based-tdd` |
| Something broken | `root-cause-isolation` |
| Ready to review | `adversarial-review` |
| Feedback received | `review-response` |
| Before "done" | `ship-gate` |
| Branch done | `completion-protocol` |
| After a cycle | `momentum-check` |
| Trivial change | `fast-track` (check criteria first) |
| Large project / multi-session | `cognitive-offload` |
| Cost concern | `cost-envelope` |
| New skill needed | `craft-skill` |

## Priority

1. User's explicit instructions (CLAUDE.md, direct requests)
2. Ironclad skills
3. Default Claude behavior
