---
name: context-management
description: Use when working on large projects with many files, when context window is filling up, when starting a new session on an existing project, or when the agent seems to forget earlier decisions
---

# Context Management

**Core principle:** Your context window is finite. Manage it like memory — load what you need, unload what you don't, and always know where to find things.

## When to Use

- Project has 10+ files or resources
- Multiple sessions needed to complete work
- Agent responses getting slower or less accurate (context full)
- Starting a new session on existing project
- Agent forgets decisions made earlier in conversation

## The Three Problems

### 1. Context Overflow
Too much loaded → agent gets confused, slow, or loses track.

### 2. Context Loss
New session → agent doesn't know what happened before.

### 3. Context Fragmentation
Information spread across many files → agent can't see the full picture.

## Project State File

**Every large project needs a `PROJECT_STATE.md` at the root.** This is the agent's memory between sessions.

```markdown
# Project State

## Current Status
- Phase: [setup/development/testing/polish]
- Last completed: [task/resource name]
- Next up: [what to do next]
- Blockers: [any issues]

## Architecture Decisions
- [Decision 1]: [why]
- [Decision 2]: [why]

## Completed Resources
- [x] resource-name — working, tested
- [x] resource-name — working, needs polish

## In Progress
- [ ] resource-name — [current status]

## Known Issues
- [issue 1]: [workaround or plan]

## File Map
- `path/to/important/file` — [what it does]
- `path/to/config` — [what it configures]
```

**Update this file after every significant change.** It costs 30 seconds and saves 10 minutes of re-discovery next session.

## Session Handoff

When ending a session:

1. Update PROJECT_STATE.md
2. Commit with message: `chore: update project state — [brief summary]`
3. List any decisions that aren't captured in code/docs

When starting a new session:

1. Read PROJECT_STATE.md FIRST
2. Check git log for recent changes: `git log --oneline -10`
3. Verify current state matches what PROJECT_STATE says
4. Resume from "Next up"

## Context Window Strategy

### Load Tiers

**Always loaded (Tier 1):** PROJECT_STATE.md, current task spec, file being edited. ~10-20% of context.

**Load on demand (Tier 2):** Related files, test files, configs. Load when needed, summarize and unload after.

**Never load whole (Tier 3):** Large reference docs, full database schemas, entire codebases. Load only relevant sections.

### Practical Rules

- **Don't load files "just in case"** — load when you need them
- **Summarize large files** after reading — keep the summary, unload the file
- **One resource at a time** — finish it, commit, then move to next
- **Don't re-read files** you already processed in this session unless they changed

### When Context is Getting Full

Signs: agent responses slower, repeating itself, forgetting earlier decisions.

Action:
1. Commit current work
2. Update PROJECT_STATE.md
3. Start fresh session
4. Load only what's needed for next task

## Multi-Resource Projects

For projects with many resources (like a FiveM server):

### Resource Dependency Map

Keep in PROJECT_STATE.md:
```markdown
## Resource Dependencies
mythic-jobs → mythic-base, mythic-characters
mythic-shops → mythic-inventory, mythic-economy
mythic-drugs → mythic-inventory, mythic-police, mythic-economy
```

This tells the agent what to load when working on a specific resource.

### Work Order

Build resources in dependency order:
1. Core/standalone resources first
2. Resources that depend on those
3. Resources that connect everything

Don't jump between unrelated resources — finish one before starting the next.

## Checkpointing

Every 3-5 tasks or 30 minutes:
- `git add -A && git commit -m "checkpoint: [what's done]"`
- Quick PROJECT_STATE.md update
- Verify tests still pass

This creates recovery points if anything goes wrong.

## Red Flags

Rationalizing? See `using-ironclad/anti-rationalization-reference.md`.

- "I'll remember this" → You won't next session. Write it down.
- "Let me load everything first" → You'll overflow. Load on demand.
- "I don't need to commit yet" → Commit often. Uncommitted work is lost work.
- "I'll update the state file later" → Later never comes. Do it now.
- "I can hold all this in context" → If the project is big enough for this skill, you can't.
