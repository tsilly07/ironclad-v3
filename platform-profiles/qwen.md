# Qwen Models Platform Profile

## Available Skills (Full Set)
All 20 skills enabled. Qwen models support standard markdown interpretation and tool execution paths.

## Platform-Specific Notes
- Apply the rules from `QWEN.md` or `AGENTS.md` as System Prompts or Custom Instructions in your Qwen client settings if available.
- If automatic workspace context injection is not completely seamless, manually start your sessions with: "Review AGENTS.md and run the activate sequence."
- Ensure that if parallel tool calling isn't strictly supported by the particular Qwen model version being used, `parallel-execution` is emulated via step-by-step sequential tool calls.

## Recommended Defaults
- Execution: `parallel-execution` 
- Review: `adversarial-review` → `ship-gate` after each task
- Branch management: `branch-strategy` with standard Git commands
- Multi-session: `cognitive-offload` with `.ironclad/memory/`
