# Changelog

## v3.0.0 — Ironclad

Initial public release. A disciplined, token-efficient fork of the agentic skills methodology.

### Core Workflow (13 skills)
- brainstorming, writing-plans, test-driven-development
- subagent-driven-development, executing-plans, iteration
- systematic-debugging, verification-before-completion
- requesting-code-review, receiving-code-review
- finishing-a-development-branch, using-git-worktrees
- dispatching-parallel-agents

### Ironclad Originals (6 new skills)
- **error-recovery** — Stop loops, revert, change strategy
- **context-management** — PROJECT_STATE.md, load tiers, session handoff
- **token-budget** — Cost tracking per task, budget tiers, spending alerts
- **progressive-disclosure** — 3-tier context loading for large projects
- **quick-fix** — Bypass full workflow for trivial changes (<20 lines)
- **using-ironclad** — Bootstrap skill with anti-rationalization reference

### Meta
- writing-skills — Create and test new skills

### Design Principles
- 20 skills in ~1,700 lines — lean, no bloat
- One central anti-rationalization reference
- Adaptive review depth based on task complexity
- Platform profiles for Claude Code, Cursor, Codex
