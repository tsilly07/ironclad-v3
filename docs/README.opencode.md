# Ironclad on OpenCode

## Installation

```bash
git clone https://github.com/tsilly07/ironclad-v3.git
cp -r ironclad-v3/skills/ ~/your-workspace/skills/
```

OpenCode discovers skills automatically from the workspace directory.

## Notes

- `activate` is the entry skill — OpenCode should invoke it at session start
- `cognitive-offload` memory stored in `.ironclad/memory/` within your workspace
- All core workflow skills are fully compatible
- Tool names may differ from Claude Code — check `platform-profiles/` for mappings
