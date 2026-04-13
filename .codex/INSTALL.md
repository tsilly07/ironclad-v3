# Ironclad — Codex Installation

## Quick Install

```bash
git clone https://github.com/tsilly07/ironclad-v3.git
cp -r ironclad-v3/skills/ ~/.agents/skills/
```

## Verification

Start a new Codex session and ask to build something. The agent should invoke `activate` and follow the intention-mapping workflow before writing any code.

## Platform Notes

See `platform-profiles/codex.md` for Codex-specific skill adaptations and limitations.

## Updating

```bash
cd ironclad-v3 && git pull
cp -r skills/ ~/.agents/skills/
```
