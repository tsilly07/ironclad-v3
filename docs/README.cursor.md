# Ironclad on Cursor

## Installation

In Cursor Agent chat:
```
/add-plugin ironclad
```

Or clone locally and point Cursor to the directory:
```bash
git clone https://github.com/tsilly07/ironclad-v3.git
```

## Notes

- `cognitive-offload` memory stored in `.ironclad/memory/` in your workspace
- `intention-mapping` works well in Composer mode
- `swarm` not fully available — use `parallel-execution` for multi-phase work
- See `platform-profiles/cursor.md` for specific configuration

## Updating

```
/update-plugin ironclad
```
