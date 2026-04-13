# Ironclad — Claude Code Configuration

This project uses the Ironclad agentic skills framework.

## Rules

1. **Always invoke skills before responding.** Use the `Skill` tool, not the `Read` tool.
2. **Follow the using-ironclad skill** at the start of every conversation.
3. **Never skip a skill** because a task "seems simple." Check first.
4. **TDD is mandatory.** Write failing test → minimal code → refactor. No exceptions.
5. **Error recovery over retry loops.** If stuck 3+ times, invoke error-recovery.
6. **Track context.** Use PROJECT_STATE.md for multi-session projects.
7. **Respect token budgets.** Don't re-read files, batch changes, skip verbose output.

## Skill Priority

Process skills first (brainstorming, debugging) → Implementation skills second.

## Quick Reference

- Design before code → `brainstorming`
- Break into tasks → `writing-plans`
- Execute tasks → `subagent-driven-development` or `executing-plans`
- Stuck in a loop → `error-recovery`
- Trivial change → `quick-fix`
- Large project → `context-management` + `progressive-disclosure`
- Cost concerns → `token-budget`
