---
name: using-ironclad
description: Use when starting any conversation - establishes how to find and use skills, requiring Skill tool invocation before ANY response including clarifying questions
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY-IMPORTANT>

## Instruction Priority

1. **User's explicit instructions** (CLAUDE.md, GEMINI.md, AGENTS.md, direct requests) — highest
2. **Ironclad skills** — override default system behavior
3. **Default system prompt** — lowest

## How to Access Skills

**Claude Code:** Use the `Skill` tool. Never use the Read tool on skill files.
**Copilot CLI:** Use the `skill` tool. Auto-discovered from installed plugins.
**Gemini CLI:** Use `activate_skill` tool. Loaded at session start.
**Other:** Check platform docs.

Non-CC platforms: see `references/copilot-tools.md` or `references/codex-tools.md` for tool equivalents.

## The Rule

**Invoke relevant skills BEFORE any response or action.** Even 1% chance = invoke. If it turns out wrong, you don't need to use it.

## Red Flags

Rationalizing skipping a skill? See `anti-rationalization-reference.md` in this directory. Key triggers:

- "This is just a simple question" → Questions are tasks. Check for skills.
- "I need more context first" → Skill check comes BEFORE clarifying questions.
- "I can check git/files quickly" → Check for skills first.
- "The skill is overkill" → Simple things become complex. Use it.
- "I remember this skill" → Skills evolve. Read current version.

## Skill Priority

1. **Process skills first** (brainstorming, debugging) — determine HOW to approach
2. **Implementation skills second** — guide execution

## Skill Types

**Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.
**Flexible** (patterns): Adapt principles to context.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.
