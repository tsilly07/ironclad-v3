---
name: brainstorming
description: "You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation."
---

# Brainstorming Ideas Into Designs

Help turn ideas into fully formed designs through collaborative dialogue.

<HARD-GATE>
Do NOT invoke any implementation skill, write any code, or take any implementation action until you have presented a design and the user has approved it. This applies to EVERY project regardless of perceived simplicity.
</HARD-GATE>

**Anti-Pattern:** "This is too simple to need a design." Every project goes through this process. The design can be short, but you MUST present it and get approval.

## Checklist

Complete in order:

1. **Explore project context** — check files, docs, recent commits
2. **Offer visual companion** (if visual questions ahead) — own message, not combined with questions. See Visual Companion section.
3. **Ask clarifying questions** — one at a time, understand purpose/constraints/success criteria
4. **Propose 2-3 approaches** — with trade-offs and your recommendation
5. **Present design** — in sections scaled to complexity, get approval after each
6. **Write design doc** — save to `docs/ironclad/specs/YYYY-MM-DD-<topic>-design.md`, commit
7. **Spec self-review** — check for placeholders, contradictions, ambiguity, scope. Fix inline.
8. **User reviews written spec** — ask user to review before proceeding
9. **Transition** — invoke writing-plans skill. **ONLY writing-plans. No other skill.**

## The Process

**Understanding the idea:** Check project state first. Before detailed questions, assess scope — if multiple independent subsystems, flag for decomposition. Ask questions one at a time, prefer multiple choice. Focus on purpose, constraints, success criteria.

**Exploring approaches:** Propose 2-3 with trade-offs. Lead with your recommendation and reasoning.

**Presenting the design:** Scale each section to its complexity (few sentences if straightforward, up to 200-300 words if nuanced). Ask after each section. Cover: architecture, components, data flow, error handling, testing.

**Design for isolation:** Break into smaller units with one clear purpose, well-defined interfaces, testable independently. Smaller units = better reasoning and more reliable edits.

**Working in existing codebases:** Follow existing patterns. Include targeted improvements where existing code has problems that affect the work, but don't propose unrelated refactoring.

## After the Design

**Documentation:** Save to `docs/ironclad/specs/YYYY-MM-DD-<topic>-design.md`. Commit.

**Spec Self-Review:** Placeholder scan, internal consistency, scope check, ambiguity check. Fix inline.

**User Review Gate:** Ask user to review spec. Wait for approval. If changes requested → make them. Only proceed once approved.

**Implementation:** Invoke writing-plans. Do NOT invoke any other skill.

## Key Principles

- One question at a time
- Multiple choice preferred
- YAGNI ruthlessly
- Always propose 2-3 approaches
- Incremental validation

## Visual Companion

When upcoming questions involve visual content (mockups, diagrams, layouts), offer once:

> "Some of what we're working on might be easier to show in a browser. I can put together mockups, diagrams, comparisons. Want to try it?"

**This offer MUST be its own message.** No other content. Per-question decision: use browser for content that IS visual (mockups, wireframes, layout comparisons). Use terminal for text content (requirements, conceptual choices, tradeoff lists).

If accepted, read `skills/brainstorming/visual-companion.md` before proceeding.
