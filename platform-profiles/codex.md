# Codex Platform Profile

## Available Skills
All skills enabled with the following adaptations:

## Disabled/Modified Skills
- **dispatching-parallel-agents** — Codex runs single-agent. Use executing-plans for sequential execution.
- **using-git-worktrees** — Codex operates in sandboxed environments. Skip worktree creation; work in provided workspace.
- **subagent-driven-development** — No subagent dispatch in Codex. Use executing-plans exclusively.

## Platform-Specific Notes
- Skills discovered via `~/.agents/skills/` directory
- Install: follow `.codex/INSTALL.md`
- See `references/codex-tools.md` for tool name equivalents
- Codex applies changes as PRs — finishing-a-development-branch defaults to Option 2 (PR)

## Recommended Defaults
- Execution: executing-plans (only option without subagents)
- Review: Self-review only (no subagent reviewers available)
- Worktrees: Not applicable
- Iteration: Use iteration skill with Patch mode for quick fixes
