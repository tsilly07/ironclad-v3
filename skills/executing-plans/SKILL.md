---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** Ironclad works much better with subagent access. If subagents are available, use ironclad:subagent-driven-development instead.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically — identify questions or concerns
3. If concerns: raise with human partner before starting
4. If clear: create TodoWrite and proceed

### Step 2: Execute Tasks
For each task: mark in_progress → follow each step exactly → run verifications → mark completed.

### Step 3: Complete Development
After all tasks verified:
- **REQUIRED SUB-SKILL:** Use ironclad:finishing-a-development-branch

## When to Stop

**STOP immediately when:** Hit a blocker, plan has critical gaps, you don't understand an instruction, verification fails repeatedly. Ask for clarification rather than guessing.

## Remember
- Review plan critically first
- Follow steps exactly, don't skip verifications
- Stop when blocked, don't guess
- Never start on main/master without explicit user consent

## Integration

**Required:** ironclad:using-git-worktrees, ironclad:writing-plans, ironclad:finishing-a-development-branch
