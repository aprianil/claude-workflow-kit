# Workflow Rules

These rules apply to every session. They are non-negotiable unless explicitly
told to skip them.

## Plan Before Work

Before implementing any non-trivial feature or fix:
1. Write a brief in `.claude/briefs/` using the template there
2. Research the codebase first — find specific files, line numbers, code snippets
3. Research external docs and best practices
4. Present the plan for approval before writing any code

Skip only for: bug fixes under 10 lines, purely mechanical changes.

## Context Management

- Don't research and build in the same session. Research outputs a brief.
  A separate fresh context implements it.
- If the conversation is getting long and outputs are degrading, don't retry —
  compact what you know into a brief and suggest starting fresh.
- Sub-agents are for context control. Use them when research would pollute
  the main context with too many tool results.

## Self-Review Before Presenting Output

Before presenting any plan, feature implementation, or design decision, answer
these three questions unprompted:
1. **Hardest decision** — What trade-off did I make? Don't hide judgment calls.
2. **Rejected alternatives** — What other approaches did I consider and why not?
3. **Lowest confidence** — What am I least sure about? Where should human
   attention focus?

Keep it brief — 1-2 sentences each. Skip for trivial changes.

## Compound After Shipping

After completing any non-trivial feature or fix, prompt: "Want to run the
compound step?" Then ask:
1. What did the agent get wrong that needed correction? → update existing
   feedback memory or constraints file
2. What reusable pattern emerged? → add to existing docs
3. What context for someone coming back in 2 weeks? → session log

Output = edits to existing files. Never create new compound docs.
