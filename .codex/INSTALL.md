# Ironclad — Codex Installation

## Quick Install

```bash
git clone https://github.com/YOUR_USERNAME/ironclad.git
cp -r ironclad/skills/ ~/.agents/skills/
```

## Verification

Start a new Codex session and ask to build something. The agent should invoke `using-ironclad` and follow the brainstorming workflow before writing any code.

## Platform Notes

See `platform-profiles/codex.md` for Codex-specific tool mappings and limitations.

## Updating

```bash
cd ironclad && git pull
cp -r skills/ ~/.agents/skills/
```
