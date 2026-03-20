---
name: Parallel agent workflow with git worktrees
description: How to run two Claude Code agents in parallel on the same repo using git worktrees and scoped briefs. Grounded in context engineering — parallel agents are for context control, not just speed.
type: project
---

## Why Parallel Agents

Parallel agents are primarily a **context control** technique. Each agent gets a
clean, focused context window with only the information relevant to its workstream.
This avoids the "dumb zone" (quality degradation at ~40% context) that happens when
one agent tries to hold an entire feature in its head.

Speed is a side effect. The real win is better output from focused context.

## Workflow: Research → Plan → Split → Implement → Compound

### 1. Research (single agent)
Before splitting, one agent researches the full feature — codebase patterns, files
involved, external docs. Outputs a shared plan with file:line references and code
snippets. This is the "compression of truth" step.

### 2. Plan (human reviews)
Human reviews the shared plan before any splitting happens. This is where you catch
wrong direction. If the plan is wrong, both agents will build the wrong thing in
parallel — twice the waste.

### 3. Split into scoped briefs
Create one brief per agent in `.claude/briefs/`. Each brief must:
- Reference the shared plan: "FIRST: Read the full plan at [path]"
- Define exact scope: files to touch, files NOT to touch
- Include API contracts or interfaces between the workstreams
- Provide mock data if one agent depends on the other's output
- Be detailed enough that a weaker model could execute without questions

The plan quality test: if the brief says "build the frontend for this feature" —
that's a wish, not a brief. If it says "create `components/widget.js`, render a
list from the `/api/widgets` endpoint (shape: `{id, name, status}`), follow the
card pattern in `components/item.js:24-45`" — that's executable.

### 4. Setup
```bash
git worktree add ../<project>-workstream-b feature/<branch-name>
```
- Agent A works in the main directory, Agent B in the worktree
- Each on its own branch, separate directories, zero shared state
- Copy `.env` and run install commands in the worktree — untracked files don't
  carry over
- Use different ports per agent to avoid conflicts (add to brief)
- Commit briefs on BOTH branches — they don't auto-share across worktrees

### 5. Implement (parallel, fresh contexts)
Each agent starts a fresh session with only its scoped brief loaded. This is
critical — don't reuse the research session. Fresh context = smart zone.

### 6. Compound (after merging)
After both workstreams merge, run the compound step:
- What did each agent get wrong? → feedback memory
- Did the API contract hold, or did it need adjustment? → update brief template
- Was the scope split clean, or did work overlap? → lesson for next split

## Key Lessons

- **Don't force parallelism.** If the work is tightly coupled (changes in one
  file depend on changes in another), keep it in one agent. Forced splits create
  merge conflicts and coordination overhead that outweigh the context benefits.
- **Commit before switching.** Changes leak between workspaces if uncommitted.
- **The shared plan is the contract.** Both agents reference it. If one agent
  needs to deviate, the plan should be updated — not silently ignored.
- **Sub-agents are not roles.** Don't think "frontend agent" and "backend agent."
  Think "agent with context about files A, B, C" and "agent with context about
  files X, Y, Z." The split is about context, not identity.

## When to Use

Use when a feature cleanly splits into independent workstreams — backend/frontend,
API/UI, data layer/presentation, two unrelated modules. Don't use when:
- The workstreams share files or have circular dependencies
- The feature is small enough for one focused context
- You'd spend more time writing briefs than you'd save in parallel execution
