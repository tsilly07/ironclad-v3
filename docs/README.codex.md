# Ironclad on Codex

## Installation

```bash
git clone https://github.com/YOUR_USERNAME/ironclad.git
cp -r ironclad/skills/ ~/.agents/skills/
```

## Platform Limitations

- Codex runs in single-agent mode — `dispatching-parallel-agents` is not available
- Git worktrees may not be supported depending on environment
- Subagent-driven-development works but without true subagent isolation

## Recommended Skills

Focus on: brainstorming, writing-plans, executing-plans, TDD, systematic-debugging, error-recovery, quick-fix.

See `platform-profiles/codex.md` for full details.
