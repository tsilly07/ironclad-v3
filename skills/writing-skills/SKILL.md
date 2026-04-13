---
name: writing-skills
description: Use when creating new skills, editing existing skills, or verifying skills work before deployment
---

# Writing Skills

**Writing skills IS Test-Driven Development applied to process documentation.**

**Personal skills live in agent-specific directories (`~/.claude/skills` for Claude Code, `~/.agents/skills/` for Codex)**

**REQUIRED BACKGROUND:** You MUST understand ironclad:test-driven-development before using this skill.

**Official guidance:** See anthropic-best-practices.md for Anthropic's official skill authoring best practices.

## What is a Skill?

A reusable reference guide for proven techniques, patterns, or tools. NOT a narrative about how you solved a problem once.

## When to Create

**Create when:** Technique wasn't intuitively obvious, you'd reference it across projects, pattern applies broadly, others would benefit.

**Don't create for:** One-off solutions, standard well-documented practices, project-specific conventions (put in CLAUDE.md), mechanically enforceable constraints (automate instead).

## Skill Types

- **Technique:** Concrete method with steps (condition-based-waiting, root-cause-tracing)
- **Pattern:** Mental model for problems (flatten-with-flags)
- **Reference:** API docs, syntax guides, tool documentation

## Directory Structure

```
skills/
  skill-name/
    SKILL.md              # Main reference (required)
    supporting-file.*     # Only if needed
```

Separate files for heavy reference (100+ lines) or reusable tools. Keep everything else inline.

## SKILL.md Structure

**Frontmatter (YAML):** Two required fields: `name` and `description` (max 1024 chars total). See [agentskills.io/specification](https://agentskills.io/specification).

```markdown
---
name: Skill-Name-With-Hyphens
description: Use when [specific triggering conditions and symptoms]
---
# Skill Name
## Overview — Core principle in 1-2 sentences
## When to Use — Symptoms, use cases, when NOT to use
## Core Pattern — Before/after code comparison
## Quick Reference — Table for scanning
## Common Mistakes — What goes wrong + fixes
```

## Claude Search Optimization (CSO)

### Description Field (Critical for discovery)

**Description = When to Use, NOT What the Skill Does.** Start with "Use when..." focusing on triggering conditions only.

**NEVER summarize workflow in description.** Testing showed Claude follows description shortcuts instead of reading full skill content. A description saying "code review between tasks" caused ONE review instead of the TWO the skill specified.

```yaml
# ❌ BAD: Summarizes workflow
description: Use when executing plans - dispatches subagent per task with code review between tasks

# ✅ GOOD: Just triggering conditions
description: Use when executing implementation plans with independent tasks in the current session
```

### Keywords and Naming

Use words Claude would search for: error messages, symptoms, synonyms, tool names. Name by what you DO: `condition-based-waiting` > `async-test-helpers`. Gerunds work well: `creating-skills`, `debugging-with-logs`.

### Token Efficiency

Skills load into EVERY conversation. Every token counts.

**Targets:** Getting-started: <150 words. Frequently-loaded: <200 words. Other: <500 words.

**Techniques:** Reference `--help` instead of documenting all flags. Cross-reference other skills instead of repeating. Compress examples. Eliminate redundancy.

### Cross-Referencing

Use skill name with explicit markers:
- ✅ `**REQUIRED SUB-SKILL:** Use ironclad:test-driven-development`
- ❌ `@skills/testing/test-driven-development/SKILL.md` (force-loads, burns context)

## Flowchart Usage

Use ONLY for non-obvious decision points and process loops. Never for reference material, code examples, or linear instructions. See @graphviz-conventions.dot for style rules.

## Code Examples

**One excellent example beats many mediocre ones.** Choose most relevant language. Complete, well-commented, from real scenario.

## The Iron Law

```
NO SKILL WITHOUT A FAILING TEST FIRST
```

Applies to NEW skills AND EDITS. Write skill before testing? Delete it. Start over.

## RED-GREEN-REFACTOR for Skills

### RED: Baseline
Run pressure scenario with subagent WITHOUT skill. Document exact behavior, rationalizations used, which pressures triggered violations.

### GREEN: Write Minimal Skill
Address those specific rationalizations. Don't add content for hypothetical cases. Verify agents now comply.

### REFACTOR: Close Loopholes
Agent found new rationalization? Add explicit counter. Re-test until bulletproof.

See @testing-skills-with-subagents.md for complete testing methodology.

## Bulletproofing Against Rationalization

For discipline-enforcing skills:
- Close every loophole explicitly (don't just state rule — forbid specific workarounds)
- Add "Violating the letter of the rules is violating the spirit" early
- Build rationalization table from baseline testing
- Create red flags list pointing to `using-ironclad/anti-rationalization-reference.md`

## Testing by Skill Type

| Type | Test With | Success Criteria |
|------|-----------|------------------|
| **Discipline** (TDD, verification) | Pressure scenarios, combined pressures | Follows rule under maximum pressure |
| **Technique** (condition-based-waiting) | Application + variation + gap scenarios | Successfully applies to new scenario |
| **Pattern** (reducing-complexity) | Recognition + application + counter-examples | Correctly identifies when/how to apply |
| **Reference** (API docs) | Retrieval + application + gap testing | Finds and correctly applies info |

## Anti-Patterns

- ❌ Narrative examples ("In session 2025-10-03, we found...")
- ❌ Multi-language dilution (example-js.js, example-py.py, example-go.go)
- ❌ Code in flowcharts (can't copy-paste)
- ❌ Generic labels (helper1, step3)

## Skill Creation Checklist

**RED Phase:**
- [ ] Create pressure scenarios (3+ combined pressures for discipline skills)
- [ ] Run WITHOUT skill — document baseline verbatim
- [ ] Identify rationalization patterns

**GREEN Phase:**
- [ ] Name: letters, numbers, hyphens only
- [ ] YAML frontmatter with `name` and `description`
- [ ] Description: "Use when..." + triggers/symptoms, third person, NO workflow summary
- [ ] Keywords throughout for search
- [ ] Address specific baseline failures
- [ ] One excellent example
- [ ] Run WITH skill — verify compliance

**REFACTOR Phase:**
- [ ] Identify new rationalizations → add counters
- [ ] Build rationalization table
- [ ] Red flags list referencing anti-rationalization-reference.md
- [ ] Re-test until bulletproof

**Deployment:**
- [ ] Commit and push
- [ ] Consider contributing via PR
