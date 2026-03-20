# Claude Workflow Kit

Reusable workflow system for engineering with AI agents. Copy what you need
into any project.

## What's Inside

```
.claude/
  CLAUDE.md              — Local rules (plan, compound, self-review, context mgmt)
  briefs/TEMPLATE.md     — PRD template for planning before implementation

memory/
  feedback_context_engineering.md   — Context window management principles
  feedback_plan_and_compound.md     — Plan-before-work + compound-after-shipping rules
  project_parallel_agents.md        — Parallel agents with git worktrees and scoped briefs
```

## How to Use

### For a new project:

1. Copy `.claude/CLAUDE.md` to your project's `.claude/CLAUDE.md`
2. Copy `.claude/briefs/` to your project's `.claude/briefs/`
3. Copy memory files to your Claude project memory directory
   (`~/.claude/projects/<project-path>/memory/`)
4. Add `.claude/CLAUDE.md` to `.gitignore` (it's personal workflow, not shared)

### What to customize per project:

- The briefs template — add project-specific validation criteria
- The memory files — add project-specific feedback as you work
- CLAUDE.md — add project-specific rules on top of these defaults

## Sources

- [Compound Engineering](https://every.to/guides/compound-engineering) — Plan → Work → Review → Compound loop, 50/50 rule
- [No Vibes Allowed: Solving Hard Problems in Complex Codebases](https://www.youtube.com/watch?v=rmvDxxNubIg) — Dex Horthy, context engineering, Research → Plan → Implement
