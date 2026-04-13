# Ironclad

A disciplined, token-efficient agentic skills framework for AI coding agents.

Ironclad enforces structured workflows — brainstorm before you build, test before you ship, recover before you spiral. It works with Claude Code, Cursor, Codex, OpenClaw, and any platform that supports skill-based agent instructions.

## Philosophy

- **Discipline over speed** — Process prevents mistakes. Every feature goes through design → plan → TDD → review.
- **Token efficiency** — Skills are lean. No bloat, no marketing copy, no repeated content. One central anti-rationalization reference instead of duplicates in every skill.
- **Adaptive review** — Not every task needs full two-stage review. Trivial tasks get single-pass, complex tasks get the full treatment.
- **Recovery built-in** — When the agent gets stuck, it doesn't keep burning tokens. It stops, reverts, and changes strategy.
- **Context-aware** — Large projects get managed through state files, load tiers, and session handoff protocols.

## What's Inside

### Core Workflow
| Skill | Purpose |
|-------|---------|
| **brainstorming** | Design before code. Every project gets a spec. |
| **writing-plans** | Break work into bite-sized TDD tasks |
| **subagent-driven-development** | Execute with fresh subagent per task + adaptive review |
| **executing-plans** | Sequential execution with checkpoints |
| **test-driven-development** | RED-GREEN-REFACTOR. No exceptions. |
| **using-git-worktrees** | Isolated workspaces for every feature |
| **finishing-a-development-branch** | Merge, PR, keep, or discard — structured completion |

### Quality Gates
| Skill | Purpose |
|-------|---------|
| **requesting-code-review** | Dispatch reviewer subagent with precise context |
| **receiving-code-review** | Technical evaluation, not performative agreement |
| **verification-before-completion** | Evidence before claims. Always. |
| **systematic-debugging** | 4-phase root cause process. No guessing. |

### Ironclad Originals
| Skill | Purpose |
|-------|---------|
| **iteration** | Structured refinement after first implementation cycle |
| **error-recovery** | Stop loops, revert, change strategy — save tokens |
| **context-management** | PROJECT_STATE.md, load tiers, session handoff for large projects |
| **token-budget** | Cost tracking per task, budget tiers, spending alerts |
| **progressive-disclosure** | 3-tier context loading — never overflow the context window |
| **quick-fix** | Bypass full workflow for trivial changes under 20 lines |

### Meta
| Skill | Purpose |
|-------|---------|
| **writing-skills** | Create new skills following TDD methodology |
| **using-ironclad** | Bootstrap — ensures skills are invoked before any action |
| **dispatching-parallel-agents** | Concurrent subagent workflows |

### Platform Profiles
Platform-specific configurations for Claude Code, Cursor, and Codex. Each profile disables irrelevant skills and sets recommended defaults.

## Installation

### OpenClaw
Copy `skills/` directory into your workspace:
```bash
cp -r skills/ ~/your-workspace/skills/
```

### Claude Code
```
/plugin install ironclad
```

### Cursor
```
/add-plugin ironclad
```

### Codex
Copy skills to `~/.agents/skills/`

## Design Principles

**Lean by default.** 20 skills in ~1,700 lines. Every line earned its place by being non-obvious — if the agent would do it anyway, it's not in the skill.

**One source of truth.** Anti-rationalization patterns live in one central reference file, not copy-pasted across 14 skills.

**Adaptive, not rigid.** Review depth scales with task complexity. Iteration doesn't require full re-brainstorm. Recovery doesn't require starting over.

**Platform-agnostic.** Works with any agent that reads markdown skill files. Platform profiles handle the differences.

## License

MIT
