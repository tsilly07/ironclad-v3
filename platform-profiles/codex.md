# Codex Platform Profile

## Available Skills
All skills enabled with the following adaptations:

## Disabled/Modified Skills
- **swarm** — Codex runs single-agent. Use `parallel-execution` with phase checkpoints instead.
- **branch-strategy** (worktree mode) — Codex operates in sandboxed environments. Skip worktree creation; apply branch strategy to current workspace.

## Platform-Specific Notes
- Skills discovered via `~/.agents/skills/` directory
- Install: follow `.codex/INSTALL.md`
- `completion-protocol` defaults to PR output (Codex applies changes as PRs)
- `cognitive-offload` memory stored in workspace at `.ironclad/memory/`

## Recommended Defaults
- Execution: `parallel-execution` (phase-based, single agent)
- Review: `adversarial-review` self-review (no subagent reviewers)
- Worktrees: Not applicable
- Fast changes: `fast-track` with qualification check
