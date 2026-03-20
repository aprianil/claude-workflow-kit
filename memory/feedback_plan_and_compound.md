---
name: Plan Before Work + Compound After Shipping
description: Two workflow rules — always write a brief before implementing, and always run a compound step after shipping. Route corrections into existing files, not new docs.
type: feedback
---

## Plan Before Work

Before implementing any non-trivial feature or fix, write a brief in `.claude/briefs/` using the template there. Research the codebase and external docs first. Present the plan for approval before writing code. A 5-line brief prevents 50 lines of wrong code.

**Skip this only for:** bug fixes under 10 lines or purely mechanical changes (renaming, formatting).

**Why:** Jumping straight to code means the agent explores and builds in the same context, which degrades quality. A plan separates research from implementation and gives the human a chance to catch wrong direction early.

**How to apply:** At the start of any feature task, before writing any implementation code, create a brief using the template at `.claude/briefs/TEMPLATE.md`.

## Compound After Shipping

After completing any non-trivial feature or fix, run a compound step. Ask three questions:

1. **What did the agent get wrong that needed correction?** → save as feedback memory or update an existing constraints file
2. **What reusable pattern emerged?** → add to design constraints or existing docs
3. **What context would help someone coming back to this in 2 weeks?** → add to session log

**Output should be edits to existing files, not new files.** A line added to a memory, a row in a table, a note in a session log. Don't create separate compound docs — that's bloat.

**Skip this only if:** genuinely nothing was learned.

**Why:** Each correction that gets encoded is one less correction next time. Without this step, the same mistakes repeat every session. This is the step that makes work compound — where 50/50 (features vs system improvements) actually happens.

**How to apply:** After shipping, prompt: "Want to run the compound step?" Then ask the three questions and route answers to the right place.

## Self-Review Before Presenting Output

Before presenting any significant output (a plan, a feature implementation, a design decision), answer these three questions unprompted:

1. **"What was the hardest decision I made here?"** — Surface the judgment calls. Don't hide trade-offs behind clean-looking code.
2. **"What alternatives did I reject, and why?"** — Show the options considered. This lets the human evaluate your reasoning, not just your result.
3. **"What am I least confident about?"** — Flag your own weak spots. This builds trust and directs human attention where it's most needed.

**Why:** Agent output that looks clean can still be wrong in ways that are hard to spot. These questions force the agent to expose its reasoning so the human reviews the thinking, not just the artifact. It also prevents the pattern where the agent confidently presents a bad approach and the human has to discover the problem themselves.

**How to apply:** Include answers to these three questions as a brief section at the end of any plan, significant code change, or design proposal. Keep it short — 1-2 sentences per question. Don't add this to trivial changes.
