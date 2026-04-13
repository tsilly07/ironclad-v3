# Ironclad on Codex

## Installation

```bash
git clone https://github.com/tsilly07/ironclad-v3.git
cp -r ironclad-v3/skills/ ~/.agents/skills/
```

## Platform Limitations

- Codex runs in single-agent mode — use `parallel-execution` instead of `swarm`
- Git worktrees may not be supported depending on environment
- `branch-strategy` applies to current workspace only

## Recommended Skills

Focus on: `activate`, `intention-mapping`, `task-graph`, `parallel-execution`, `confidence-based-tdd`, `adversarial-review`, `ship-gate`, `root-cause-isolation`, `fast-track`.

See `platform-profiles/codex.md` for full details.
