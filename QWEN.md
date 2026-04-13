# Ironclad — Qwen Models Configuration

This project uses the Ironclad cognitive framework for AI coding agents.

## Setup

Skills are loaded by reading the framework instructions at the start of a session.
If your Qwen model client supports workspace instructions or system prompts, paste the contents of `AGENTS.md` there.
Otherwise, begin every session by telling Qwen to read `AGENTS.md` and activate `activate`.

## Rules

Same as `CLAUDE.md` and `GEMINI.md` — invoke skills before responding, run assumption-audit before implementation, use cognitive-offload for large/multi-session projects.

## Activation Order

1. `activate` — always first
2. Identify relevant skills from the skill map in `activate/SKILL.md`
3. Invoke applicable skills before any action

## Platform Notes

See `platform-profiles/qwen.md` for Qwen-specific tool equivalents and limitations.
