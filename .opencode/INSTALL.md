# Ironclad — OpenCode Installation

## Quick Install

```bash
git clone https://github.com/tsilly07/ironclad-v3.git
cp -r ironclad-v3/skills/ ~/your-workspace/skills/
```

## Verification

Start a new session and request a feature. The agent should invoke `activate` automatically and follow the intention-mapping workflow before writing code.

## Updating

```bash
cd ironclad-v3 && git pull
cp -r skills/ ~/your-workspace/skills/
```
