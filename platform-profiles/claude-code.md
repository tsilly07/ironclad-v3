# Claude Code Platform Profile

## Available Skills (Full Set)
All 20 skills enabled. Claude Code supports subagents, git operations, and full skill tooling.

## Platform-Specific Notes
- Use `Skill` tool to invoke skills (never the Read tool on skill files)
- `swarm` is the recommended execution mode for parallel tasks
- `parallel-execution` for phase-based orchestration
- `branch-strategy` with git worktrees fully supported

## Recommended Defaults
- Execution: `parallel-execution` or `swarm` (based on task-graph parallelism)
- Review: `adversarial-review` → `ship-gate` after each task
- Branch management: `branch-strategy` with `.worktrees/` directory
- Multi-session: `cognitive-offload` with `.ironclad/memory/`
