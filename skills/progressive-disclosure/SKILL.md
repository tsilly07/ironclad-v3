---
name: progressive-disclosure
description: Use when dealing with large codebases, documentation, or reference material — load information in tiers to avoid context overflow and improve accuracy
---

# Progressive Disclosure

**Core principle:** Load the minimum context needed for the current task. Expand only when needed.

## The Problem

Loading everything at once causes: slower responses, reduced accuracy, agent confusion, wasted tokens. A 60-file FiveM server doesn't need all 60 files loaded to edit one resource.

## Three-Tier Loading

### Tier 1: Always Loaded (~5% of project)
- PROJECT_STATE.md
- Current task description
- The specific file being edited

**Never load more than this by default.**

### Tier 2: Load on Demand (~15% of project)
- Files directly imported/required by current file
- Test file for current code
- Config file for current resource
- Related component interfaces (not implementations)

**Load when:** Current task references them, or you need to understand an interface.

### Tier 3: Reference Only (~80% of project)
- Framework documentation
- Other resources not related to current task
- Database schemas
- Full API references

**Load when:** Specific question requires it. Load only the relevant section, not the whole file.

## How to Apply

### Before Reading a File, Ask:
1. Do I actually need this file for the current task?
2. Can I work from what I already know?
3. Do I need the whole file or just a specific function/section?

### When Working on a Resource:

```
LOAD: The resource's main files (client/, server/, config/)
SKIP: Other resources, framework internals, unrelated configs
REFERENCE: Framework docs only if you need a specific API
```

### When Debugging:

```
LOAD: The file with the error + its direct dependencies
SKIP: Everything else
REFERENCE: Stack trace tells you exactly where to look
```

## Summarize and Unload

After reading a large file for reference:
1. Extract what you need (function signature, config value, pattern)
2. Note it in your working memory / response
3. Don't load the file again unless it changed

```
Instead of: "Let me read the 500-line vehicle config again..."
Do: "Earlier I noted vehicles use Type 0=car, 1=boat, 2=aircraft"
```

## Framework Documentation Strategy

For large frameworks (Mythic, QBox, etc.):

**DON'T:** Load all docs at session start.
**DO:** Keep a cheat sheet of key patterns:

```markdown
## Quick Reference (always in context)
- Components: RegisterComponent / FetchComponent / ExtendComponent
- Events: TriggerServerEvent / TriggerClientEvent / RegisterNetEvent
- Callbacks: RegisterServerCallback / ServerCallback
- DB: Database.Game:find / Database.Game:findOne / Database.Game:updateOne
```

**Load full docs only when:** You need a specific API you don't have in the cheat sheet.

## Integration with Context Management

Progressive disclosure feeds into the context-management skill:
- Tier 1 = what's always in PROJECT_STATE.md
- Tier 2 = what the dependency map tells you to load
- Tier 3 = what you reference from docs only when stuck

## Red Flags

- "Let me read all the files first to understand the project" → Read only what you need NOW.
- "Loading the full documentation..." → Load only the section you need.
- "Let me check every resource to see how they do it" → Check ONE similar resource, apply pattern.
- Reading the same file twice in one session → Summarize first time, reference summary after.
