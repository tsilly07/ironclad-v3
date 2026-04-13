# Cursor Platform Profile

## Available Skills
All skills enabled with the following adaptations:

## Disabled/Modified Skills
- **swarm** — Limited subagent support in Cursor. Use `parallel-execution` for phase-based sequential execution instead.
- **branch-strategy** (worktree mode) — Cursor manages its own workspace. Skip worktree creation unless user requests explicit isolation.

## Platform-Specific Notes
- Use `/add-plugin ironclad` to install
- Skills invoke via Cursor's plugin system
- `cognitive-offload` memory stored at `.ironclad/memory/` in your workspace
- Composer mode works well with `intention-mapping` skill

## Recommended Defaults
- Execution: `parallel-execution` (inline, phase-based)
- Review: `adversarial-review` → `ship-gate`
- Worktrees: Skip unless explicitly requested
- Fast changes: `fast-track` with qualification check
