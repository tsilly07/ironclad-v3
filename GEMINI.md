# Ironclad — Gemini CLI Configuration

This project uses the Ironclad cognitive framework for AI coding agents.

## Setup

Skills are loaded via `activate_skill` tool at session start.
Begin every session by activating `activate`.

## Rules

Same as CLAUDE.md — invoke skills before responding, run assumption-audit before implementation, use cognitive-offload for large/multi-session projects.

## Activation Order

1. `activate` — always first
2. Identify relevant skills from the skill map in activate/SKILL.md
3. Invoke applicable skills before any action

## Platform Notes

See `platform-profiles/` for Gemini-specific tool equivalents and limitations.
