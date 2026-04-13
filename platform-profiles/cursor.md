# Cursor Platform Profile

## Available Skills
All skills enabled with the following adaptations:

## Disabled/Modified Skills
- **dispatching-parallel-agents** — Limited subagent support in Cursor. Use executing-plans instead for sequential execution.
- **using-git-worktrees** — Cursor manages its own workspace. Skip worktree creation; work in current workspace unless user requests isolation.

## Platform-Specific Notes
- Use `/add-plugin ironclad` to install
- Skills invoke via Cursor's plugin system
- See `references/cursor-tools.md` for tool name equivalents
- Composer mode works best with brainstorming skill

## Recommended Defaults
- Execution: executing-plans (inline, sequential)
- Review: Single-pass for most tasks (Cursor lacks efficient subagent dispatch)
- Worktrees: Skip unless explicitly requested
