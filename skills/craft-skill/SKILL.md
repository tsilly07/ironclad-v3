---
name: craft-skill
description: Use when creating a new Ironclad skill — requires a worked example before the skill is finalized, enforces evidence-driven authoring, and tests the skill on a real case before publishing.
---

# Craft Skill

**Core principle:** A skill without a worked example is a hypothesis, not a skill.

---

## When to Use

- You want to add a new skill to Ironclad
- An existing skill is consistently ignored or misapplied (needs redesign)
- A workflow has emerged organically that should be formalized

---

## What Makes a Good Skill

A skill is a **reusable workflow for a recurring problem**. It must be:

| Property | Test |
|---|---|
| **Specific** | Could you follow it without asking clarifying questions? |
| **Triggered** | Does it have clear conditions for when to use it? |
| **Evidence-based** | Is every step grounded in what actually works, not theory? |
| **Lean** | Does every line earn its place? Is anything obvious or repeated? |
| **Self-contained** | Does it reference other skills without duplicating them? |

If any property fails → the skill isn't ready yet.

---

## Step 1 — Find the Real Pattern

Before writing anything, validate that this skill addresses a real, recurring problem.

Answer these:
1. **Recurrence**: How many times have you seen this problem in the last month?
2. **Failure mode**: What specifically goes wrong when this skill isn't used?
3. **Existing coverage**: Is this already handled by an existing skill? (Check the full skill list first.)
4. **Scope**: Is this one skill or two? Skills should have exactly one purpose.

If recurrence < 3 times or an existing skill covers it: don't create a new skill. Improve the existing one.

---

## Step 2 — Write the Worked Example First

Before writing the skill itself, write a worked example. This is mandatory.

```markdown
# Worked Example: [Skill Name]

## Scenario
[Real or realistic situation where this skill is needed]

## Without This Skill
[What an agent would do without this skill, and what goes wrong]

## With This Skill
[Step-by-step walkthrough of applying this skill to the scenario]

## Outcome
[How the result differs from the "without" case]
```

The worked example reveals whether the skill is actually useful. If you can't write a compelling worked example, the skill isn't ready.

---

## Step 3 — Write the SKILL.md

Structure every skill identically:

```markdown
---
name: [skill-name]
description: [One sentence: USE WHEN [trigger] — [what it does]. This is how the agent finds this skill.]
---

# [Skill Name]

**Core principle:** [One sentence. The fundamental insight this skill is built on.]

---

## When to Use
[Specific triggers. Be precise — not "when you feel like it".]

## [Process Sections — name them after what happens, not "step 1, step 2"]

## Red Flags
[Anti-rationalization triggers specific to this skill. 3–5 max.]
```

---

## Step 4 — Leanness Pass

After the first draft, cut:

- Any sentence that states the obvious
- Any instruction the agent would do anyway without being told
- Any paragraph that duplicates content from another skill (link instead)
- Any "always remember to..." that doesn't add constraints

**Target:** 60–80% of a first draft survives. If more than 80% survives, you weren't cutting enough.

---

## Step 5 — Test the Skill on a Real Case

Before publishing, apply the skill to the scenario from Step 2. Literally follow the SKILL.md as written:

- Did any step produce confusion?
- Did any step need clarification?
- Did you want to skip any step? (If yes: either cut it or make the trigger explicit)
- Did the outcome match the worked example?

Fix anything that caused friction. This is the only quality gate that matters.

---

## Step 6 — Publish

Create the skill directory and save to `skills/[skill-name]/SKILL.md`.

Save the worked example to `skills/[skill-name]/worked-example.md`.

Update the skill index in `activate/SKILL.md` (the skill mapping table).

Commit with message: `skill: add [skill-name] — [one sentence on what it does]`

---

## Skill Maintenance

Skills become stale. Review any skill that:
- Has been consistently ignored (agent rationalizes around it)
- Has produced unexpected behavior in real usage
- Covers a problem that no longer exists

Edit → test on real case → re-publish. Same process as authoring.

---

## Red Flags

- Writing the skill before the worked example → You're writing theory, not workflow.
- "I'll add the example later" → The example validates the skill. Later = never validated.
- Skill has 20+ steps → This is two skills. Split it.
- Every step starts with "Make sure to..." → That's a checklist, not a skill. Add the decision logic.
